即插即用IPR		IRP_MJ_PNP

所有的即插即用的事件，都会引发一个即插即用管理器向WDM驱动发送一个IPR_MJ_PNP，IRP_MJ_PNP是这个IRP的主功能代码，不同的即插即用事件会有不同的子功能代码。


## 通过设备接口寻找设备

### 设备接口

在WDM驱动中，一般很少用到设备名，也很少用到符号链接，而是用一个设备接口来指定设备。设备接口其实就是一组全局标识，它是一个128位组成的数字，并能保证在全世界范围内不会产生冲突。

**引入设备接口的主要原因是避免设备名冲突，例如不同的网卡厂商可能都将自己的设备名命名为：NetCardDevice，当用户同时插入两块网卡时，就会引起冲突。**


### WDM驱动中设置接口

在WDM驱动程序中使用设备接口，首先在创建设备的时候，不能指定设备名，这样系统会为设备自动创建一个设备名，该设备名就是一个数字，新设备加入的时候，数字会依次递增。


另外不用IoCreateSymbolicLink创建符号链接，而是用IoRegisterDeviceInterface为设备创建设备链接。

当一个设备被添加到系统时，Windows查找正确的驱动程序，并调用它的DriverEntry例程。PNP管理器然后调用驱动程序的AddDevice例程，告诉它添加了一个新设备。此时程序创建自己的设备对象，即FDO。

