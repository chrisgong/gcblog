---
title: Android 4.0蓝牙介绍
date: 2016-03-20 13:26:35
tags: [Bluetooth]
catetory: Bluetooth
---


***最近一直在倒腾各种版本的蓝牙功能适配.***

***今天先介绍下蓝牙4.0连接配对流程中各种对象的作用.***

***顺带梳理下Ble从连接到写入到接收通知的流程.***

***目前市场上的穿戴设备的蓝牙4.0读写基本都是以下实现过程.***


#### 版本及权限

> 版本支持: 

Android OS API >= 18(4.3)

> 权限支持: 

```java
<uses-permission android:name="android.permission.BLUETOOTH"/>
<uses-permission android:name="android.permission.BLUETOOTH_ADMIN"/>
<uses-feature android:name="android.hardware.bluetooth_le" android:required="true"/>
//如果 android.hardware.bluetooth_le设置为false,可以安装在不支持的设备上使用，判断是否支持蓝牙4.0用以下代码就可以了
```

#### 流程图

![](http://7xs2xr.com1.z0.glb.clouddn.com/bluetooth4.0)


#### BluetoothGatt
继承BluetoothProfile，通过BluetoothGatt可以连接设备（connect）,发现服务（discoverServices），并把相应地属性返回到BluetoothGattCallback 


```java
    public void connect(Context context, BluetoothDevice device) {
    	//开启连接
        mBluetoothGatt = device.connectGatt(context, false, mBTGattCallback);
    }
```
#### BluetoothGattCharacteristic
相当于一个数据类型，它包括一个value和0~n个value的描述（BluetoothGattDescriptor）

#### BluetoothGattDescriptor
描述符，对Characteristic的描述，包括范围、计量单位等

#### BluetoothGattService
服务，Characteristic的集合。

#### BluetoothGattCallback
蓝牙设备操作结果返回。

```java
private BluetoothGattCallback mBTGattCallback = new BluetoothGattCallback() {

	//连接结果
	@Override
	public void onConnectionStateChange(BluetoothGatt gatt, int status, int newState) {
		super.onConnectionStateChange(gatt, status, newState);
		switch (newState) {
			case BluetoothProfile.STATE_CONNECTED:
				gatt.discoverServices();
				break;
			case BluetoothProfile.STATE_DISCONNECTED:
				break;
			default:
				break;
		}
	}
	
	//发现服务结果(连接完成后调用)
	@Override
	public void onServicesDiscovered(BluetoothGatt gatt, int status) {
		super.onServicesDiscovered(gatt, status);
	}

	@Override
	public void onCharacteristicRead(BluetoothGatt gatt, BluetoothGattCharacteristic characteristic, int status) {
		super.onCharacteristicRead(gatt, characteristic, status);
	}

	//写入指令结果
	@Override
	public void onCharacteristicWrite(BluetoothGatt gatt, BluetoothGattCharacteristic characteristic, int status) {
		super.onCharacteristicWrite(gatt, characteristic, status);
	}

	//蓝牙通知数据结果
	@Override
	public void onCharacteristicChanged(BluetoothGatt gatt, BluetoothGattCharacteristic characteristic) {
		super.onCharacteristicChanged(gatt, characteristic);
	}
};
```

连接调用流程:

onConnectionStateChange -> onServicesDiscovered()

- 此方法会返回指定BluetoothDevice的所有BluetoothGattService对象,通过遍历BluetoothGattService来获取所有BluetoothGattCharacteristic,然后匹配对应的UUID,实例Service,Write,Notify等Characteristic.

```java
@Override
public void onServicesDiscovered(BluetoothGatt gatt, int status) {
    super.onServicesDiscovered(gatt, status);
    if (status == BluetoothGatt.GATT_SUCCESS) {
        List<BluetoothGattService> services = gatt.getServices();
        for (BluetoothGattService service : services) {
            List<BluetoothGattCharacteristic> _gattCharacteristics = service.getCharacteristics();
            mGattCharacteristics.add(_gattCharacteristics);
        }

        for (int i = 0; i < mGattCharacteristics.size(); i++) {
            List<BluetoothGattCharacteristic> _char_list = mGattCharacteristics.get(i);
            for (int j = 0; j < _char_list.size(); j++) {
                BluetoothGattCharacteristic _char = _char_list.get(j);
                System.out.println(" ==========> bluetooth uuid: " + _char.getUuid());
                if (UUID_WRITE.equals(_char.getUuid())) {
                    mWriteCharacteristic = _char;
                    System.out.println(" ==========> write uuid: " + _char.getUuid());
                } else if (UUID_NOTIFY.equals(_char.getUuid())) {
                    mNotifyCharacteristic = _char;
                    System.out.println(" ==========> notify uuid: " + _char.getUuid());
                }
            }
        }

        if (mNotifyCharacteristic != null) {
            if (registerNotifyChar(gatt, mNotifyCharacteristic)) {
				//注册通知成功,等待写入数据
            }
        } else {
            disConnect();
			//未找到Notify Characteristic.连接失败
        }
    }
}
```

- 如需开启设备蓝牙通知回传数据,需要往获取的Notify Characteristic的0x2902 BluetoothGattDescriptor中写入BluetoothGattDescriptor.ENABLE_NOTIFICATION_VALUE来开启设备通知服务.

```java
 private boolean registerNotifyChar(BluetoothGatt gatt, BluetoothGattCharacteristic characteristic) {
            boolean set_notify = gatt.setCharacteristicNotification(characteristic, false);
            BluetoothGattDescriptor descriptor = characteristic.getDescriptor(UUID.fromString("00002902-0000-1000-8000-00805f9b34fb"));
            boolean enable_descriptor = descriptor.setValue(BluetoothGattDescriptor.ENABLE_NOTIFICATION_VALUE);
            boolean write_descriptor = gatt.writeDescriptor(descriptor);
            System.out.println(" ==========> registerNotifyChar: " + set_notify + "-" + enable_descriptor + "-" + write_descriptor);
            if (write_descriptor && enable_descriptor && set_notify) {
                return true;
            }
            return false;
 }
```

写数据调用流程

```java
//连接成功后,开始写入数据
public void write(byte[] data) {
	if (mBluetoothGatt != null) {
		mWriteCharacteristic.setValue(data);
		mBluetoothGatt.writeCharacteristic(mWriteCharacteristic);
	}
}
```

写入数据会触发如下方法:

```java
@Override
public void onCharacteristicWrite(BluetoothGatt gatt, BluetoothGattCharacteristic characteristic, int status) {
	super.onCharacteristicWrite(gatt, characteristic, status);
}
```

设备接收数据成功后,会已通知形式下发数据,回传如下方法:

```java
@Override
public void onCharacteristicChanged(BluetoothGatt gatt, BluetoothGattCharacteristic characteristic) {
	super.onCharacteristicChanged(gatt, characteristic);
	//characteristic.getValue() //蓝牙回传数据
}
```

#### BluetoothManager
通过BluetoothManager来获取BluetoothAdapter

#### BluetoothAdapter
系统蓝牙适配器. 通过此对象可以开启,关闭系统蓝牙;可以开启附近蓝牙设备扫描

```java
if (Build.VERSION.SDK_INT < 21) {
	mBluetoothAdapter.startLeScan(mLeScanCallback);
}else {
	if (mCbtScanCallback == null){
		mCbtScanCallback = new CBTScanCallback();
	}
	mLEScanner.startScan(mLeFilters, mLeSettings, mCbtScanCallback);
}
```

#### LeScanCallback
OS API < 21 蓝牙扫描结果回调类

```java
@Override
public void onLeScan(BluetoothDevice device, int rssi, byte[] scanRecord) {
	//device: 扫描设备结果对象, rssi: 信号强弱值
}
```

#### ScanCallback
OS API >= 21 蓝牙扫描结果回调类

```java
@Override
public void onScanResult(int callbackType, ScanResult result) {
	super.onScanResult(callbackType, result);
	//result.getDevice(): 扫描设备对象,  (short)result.getRssi(): 信号强弱值
}
```

#### ScanSettings
OS API >= 21 蓝牙扫描设置类

```java
ScanSettings mLeSettings = new ScanSettings.Builder().setScanMode(ScanSettings.SCAN_MODE_LOW_LATENCY).build()
//SCAN_MODE_LOW_POWER
//SCAN_MODE_BALANCED
//SCAN_MODE_LOW_LATENCY
//SCAN_MODE_OPPORTUNISTIC
//具体各自解释见api文档
```
#### BluetoothProfile
一个通用的规范，按照这个规范来收发数据。

