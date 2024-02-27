=================
bluetoothctl-gatt
=================

-------------------------
Generic Attribute Submenu
-------------------------

:Version: BlueZ
:Copyright: Free use of this software is granted under ther terms of the GNU
            Lesser General Public Licenses (LGPL).
:Date: November 2022
:Manual section: 1
:Manual group: Linux System Administration

SYNOPSIS
========

**bluetoothctl** [--options] [gatt.commands]


Generic Attribute Commands
==========================
The Generic Attribute Profile (GATT)  defines how two connected Bluetooth LE devices can share data using concepts called Services and Characteristics. 
Data is orgnaized into a hierarchical structure of profiles, services, and characteristics in the GATT database. The device containing the GATT database is known as the server device and the device accessing the GATT database is the client device. Only the database's services and characteristics are documented here. 

Services and characteristics are differentiated by Universally Unique Identifiers (UUIDs). For GATT there are several formats of UUIDs: 128-bit, 32-bit, and 16-bit.

128-bit UUIDs: Displayed in five groups of 32 hexidecimal digits separated by hyphens (e.g., 123e4567-e89b-12d3-a456-426614174000). 128-bit UUIDs allow for custom services and characteristics that do not conflict with those of other vendors or standard profiles defined by the Bluetooth SIG.

32-bit UUIDs: A subset of the 128-bit UUID, where only 32 bits are used to represent the UUID. The UUID values fall within a certain range defined by the Bluetooth specification. The 32-bit value is inserted into a base UUID (UUUUUUUU-0000-1000-8000-00805F9B34FB) to form a full 128-bit UUID. The UUUUUUUU in the base UUID is replaced by the 32-bit value. This format is used within the Bluetooth protocol for standardized services and profiles.

16-bit UUIDs: Similar to the 32-bit UUID, but more common. This is also used for standardized services, characteristics, and descriptors defined by the Bluetooth SIG.  The 16-bit value is inserted into a base UUID (0000UUUU-0000-1000-8000-00805F9B34FB) to form a full 128-bit UUID. 

Services: Collections of related data that together perform a specific function or feature of the device. Each service contains one or more characteristics.

Characteristics: Pieces of data under a service, representing a specific piece of information or a function of the device. Each characteristic can have its own unique value, associated properties and access permissions that define how it can be interacted with. These properties include read, write, notify, indicate, and more. 

Read Property: Allows a connected device to read the value of a characteristic. Information meant to be consumed but not altered by the client device, such as the device's battery level or sensor readings. Characteristics marked with the read property are essentially broadcasting their data to be accessed by other devices, ensuring that the information can be consumed without being modified.

Write Property: Enables a connected device to change the value of a characteristic. An example of when a client device needs to alter data on the server device is a Bluetooth controlled light bulb, where the characteristic controlling the color or brightness of the light would have the write property, allowing external devices to modify these settings.

Read/Write Property: Characteristics can also be configured to support both read and write properties simultaneously. This dual capability is useful for characteristics that hold information which not only needs to be displayed to external devices but also can be updated by them. For example, a Bluetooth thermostat may have a characteristic with read/write properties for the target temperature, enabling devices to both monitor the current setting and adjust it as needed.

Notifications and Indications: In addition to read and write properties, Bluetooth characteristics can also be configured to notify or indicate to connected devices when their value has changed. This is particularly useful for dynamic data that changes over time, such as sensor measurements or device status updates.

Notifications: When a characteristic's notify property is enabled, it can send an update to connected devices whenever its value changes. This allows for real-time data streaming without the client having to continuously read the characteristic's value. Notifications are unacknowledged, meaning the sender does not receive confirmation that the message has been received successfully.

Indications: Similar to notifications, indications inform connected devices about changes in a characteristic's value. The key difference is that indications are acknowledged; the receiver sends back a confirmation to the sender upon successfully receiving the update. This ensures reliable delivery of important data changes but at the cost of higher power consumption and potentially lower throughput due to the additional confirmation step.


list-attributes
---------------

List attributes.

:Usage: **# list-attributes <attribute/UUID> <dev/local>**

select-attribute
----------------

Select attribute.

:Usage: **# select-attribute <attribute/UUID>**

attribute-info
--------------

Select attribute.

:Usage: **# attribute-info [attribute/UUID]**

read
----

Read attribute value.

:Usage: **# read [offset]**

write
-----

Write attribute value.

:Usage: **# write <data=xx xx ...> [offset] [type]**

acquire-write
-------------

Acquire Write file descriptor.

:Usage: **# acquire-write**

release-write
-------------

Release Write file descriptor.

:Usage: **# release-write**

acquire-notify
--------------

Acquire Notify file descriptor.

:Usage: **# acquire-notify**

release-notify
--------------

Release Notify file descriptor.

:Usage: **# release-notify**

notify
------

Notify attribute value.

:Usage: **# notify <on/off>**

clone
-----

Clone a device or attribute.

:Usage: **# clone [dev/attribute/UUID]**

register-application
--------------------

Register application.

:Usage: **# register-application [UUID ...]**

unregister-application
----------------------

Unregister application

:Usage: **# unregister-application**

register-service
----------------

Register application service.

:Usage: **# register-service <UUID> [handle]**

unregister-service
------------------

Unregister application service

:Usage: **# unregister-service <UUID/object>**

register-includes
-----------------

Register as Included service.

:Usage: **#r egister-includes <UUID> [handle]**

unregister-includes
-------------------

Unregister Included service.

:Usage: **# unregister-includes <Service-UUID><Inc-UUID>**

register-characteristic
-----------------------

Register service characteristic.

:Usage: **# register-characteristic <UUID> <Flags=read,write,notify...> [handle]**

unregister-characteristic
-------------------------

Unregister service characteristic.

:Usage: **# unregister-characteristic <UUID/object>**

register-descriptor
-------------------

Register characteristic descriptor.

:Usage: **# register-descriptor <UUID> <Flags=read,write...> [handle]**

unregister-descriptor
---------------------

Unregister characteristic descriptor.

:Usage: **# unregister-descriptor <UUID/object>**

RESOURCES
=========

http://www.bluez.org

REPORTING BUGS
==============

linux-bluetooth@vger.kernel.org
