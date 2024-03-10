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

Components of a GATT database

The Generic Attribute Profile (GATT)  defines how two connected Bluetooth LE devices can share data using concepts called Services and Characteristics. 
Data is orgnaized into a hierarchical structure of profiles, services, and characteristics in the GATT database. The device containing the GATT database is known as the server device and the device accessing the GATT database is the client device. Only the database's services and characteristics are documented here. 

Services and characteristics are differentiated by Universally Unique Identifiers (UUIDs). For GATT there are several formats of UUIDs: 128-bit, 32-bit, and 16-bit.

128-bit UUIDs: Displayed in five groups of 32 hexidecimal digits separated by hyphens (e.g., 123e4567-e89b-12d3-a456-426614174000). 128-bit UUIDs allow for custom services and characteristics that do not conflict with those of other vendors or standard profiles defined by the Bluetooth SIG.

32-bit UUIDs: A subset of the 128-bit UUID, where only 32 bits are used to represent the UUID. The UUID values fall within a certain range defined by the Bluetooth specification. The 32-bit value is inserted into a base UUID (UUUUUUUU-0000-1000-8000-00805F9B34FB) to form a full 128-bit UUID. The UUUUUUUU in the base UUID is replaced by the 32-bit value. This format is used within the Bluetooth protocol for standardized services and profiles.

16-bit UUIDs: Similar to the 32-bit UUID, but more common. This is also used for standardized services, characteristics, and descriptors defined by the Bluetooth SIG.  The 16-bit value is inserted into a base UUID (0000UUUU-0000-1000-8000-00805F9B34FB) to form a full 128-bit UUID. 

Services: Collections of related data that together perform a specific function or feature of the device. Each service is identified by a unique UUID and contains Characteristics that define the actual data and behaviors of the service.

Characteristics: Pieces of data under a service, representing a specific piece of information or a function of the device. Each characteristic can have its own unique value, associated properties and access permissions that define how it can be interacted with. These properties include read, write, notify, indicate, and more.  A Characteristic may include one or more Descriptors that provide additional metadata or control specific aspects of the Characteristic's behavior.

Read Property: Allows a connected device to read the value of a characteristic. Information meant to be consumed but not altered by the client device, such as the device's battery level or sensor readings. Characteristics marked with the read property are essentially broadcasting their data to be accessed by other devices, ensuring that the information can be consumed without being modified.

Write Property: Enables a connected device to change the value of a characteristic. An example of when a client device needs to alter data on the server device is a Bluetooth controlled light bulb, where the characteristic controlling the color or brightness of the light would have the write property, allowing external devices to modify these settings.

Read/Write Property: Characteristics can also be configured to support both read and write properties simultaneously. This dual capability is useful for characteristics that hold information which not only needs to be displayed to external devices but also can be updated by them. For example, a Bluetooth thermostat may have a characteristic with read/write properties for the target temperature, enabling devices to both monitor the current setting and adjust it as needed.

Notifications and Indications: In addition to read and write properties, Bluetooth characteristics can also be configured to notify or indicate to connected devices when their value has changed. This is particularly useful for dynamic data that changes over time, such as sensor measurements or device status updates.

Notifications: When a characteristic's notify property is enabled, it can send an update to connected devices whenever its value changes. This allows for real-time data streaming without the client having to continuously read the characteristic's value. Notifications are unacknowledged, meaning the sender does not receive confirmation that the message has been received successfully.

Indications: Similar to notifications, indications inform connected devices about changes in a characteristic's value. The key difference is that indications are acknowledged; the receiver sends back a confirmation to the sender upon successfully receiving the update. This ensures reliable delivery of important data changes but at the cost of higher power consumption and potentially lower throughput due to the additional confirmation step.

Organization of a GATT database
The Bluetooth GATT database is a structured collection of values that are used to share information between two Bluetooth Low Energy (LE) devices. The GATT database is organized as a hierarchical structure composed of Services, Characteristics, and Descriptors. The two LE devices share the database information through read, writes, and subscription to notifications or indications of data values. The two LE devices perform this sharing by referencing 16-bit indexes known as "handles" which are numerical identifiers for each element in the GATT database. This avoids the need to use the UUIDs to reference the elements in the database.

Service: A GATT database service is a collection of related functionalities that a device offers. It groups together one or more Characteristics that are logically related to perform a specific function or feature of the server device. Each service is identified by a unique UUID (Universally Unique Identifier) and contains Characteristics that define the actual data and behaviors of the service. 

Characteristic: Each characteristic can have its own unique value, associated properties, and access permissions that define how it can be interacted with. These properties include read, write, notify, indicate, and more. A Characteristic may include one or more Descriptors that provide additional metadata or control specific aspects of the Characteristic's behavior.

Descriptors: A descriptor is an optional attribute that describes or controls certain aspects of a Characteristic's behavior. They provide additional context or metadata about a Characteristic. Some common types of Descriptors include the Characteristic User Description (provides human-readable description), the Characteristic Presentation Format (defines the format of the Characteristic value), and the Client Characteristic Configuration (used to enable or disable notifications or indications for a Characteristic).

Handles: Every element in the GATT database (Service, Characteristic, Descriptor) is assigned a unique handle. Handles are 16-bit numerical identifiers that are used to reference these elements in GATT operations. The handles are used to avoid the need to use the UUIDs to reference the elements in the database.

list-attributes
---------------

Lists the attributes of either the local device or a remote device, encompassing services, characteristics, and handles. This command provides a comprehensive overview of the available Bluetooth attributes, which can be further interacted with using other commands.

:Usage: **# list-attributes <attribute/UUID> <dev/local>**

select-attribute
----------------

Selects a specific attribute on either the local or remote device for subsequent operations. Before you can read or write to an attribute, you must select it with this command. This establishes a context for many other commands (read, write, notify, etc.), specifying the attribute they should operate on.

:Usage: **# select-attribute <attribute/UUID/local>**

attribute-info
--------------

Displays detailed information about an attribute. If no attribute or UUID is specified, it shows information about the currently selected attribute. This command is useful for understanding the properties and capabilities of an attribute.

:Usage: **# attribute-info [attribute/UUID]**

read
----

Reads the value of an attribute. Requires that an attribute be selected beforehand with select-attribute. The optional offset parameter can be used for attributes that allow partial reads.

:Usage: **# read [offset]**

write
-----

Writes a value to an attribute. This command necessitates that an attribute be selected in advance using select-attribute. Data must be provided in hexadecimal format. Optional offset and type parameters can accommodate specific write requirements.

:Usage: **# write <data=xx xx ...> [offset] [type]**

acquire-write
-------------

Acquires a Write file descriptor for a previously selected attribute. This is useful for applications that need a file descriptor to perform write operations.

:Usage: **# acquire-write**

release-write
-------------

Releases the Write file descriptor acquired with acquire-write. This command is necessary to clean up resources after you're done with the write operation.

:Usage: **# release-write**

acquire-notify
--------------

Acquires a Notify file descriptor for a previously selected attribute. This enables applications to listen for notifications on attribute value changes.

:Usage: **# acquire-notify**

release-notify
--------------

Releases the Notify file descriptor obtained with acquire-notify. Ensures resources are freed once notification listening is no longer needed.

:Usage: **# release-notify**

notify
------

Enables or disables notifications for attribute value changes. Before this command can be used, the relevant attribute must be selected. This command allows applications to be notified of attribute changes without polling.

:Usage: **# notify <on/off>**

clone
-----

Creates a clone of a device or attribute. This can be useful for creating a backup or working with a copy for testing purposes.

:Usage: **# clone [dev/attribute/UUID]**

register-application
--------------------

Registers a new application with the Bluetooth system, allowing for the management of services, characteristics, and descriptors under this application.

:Usage: **# register-application [UUID ...]**

unregister-application
----------------------

Removes a previously registered application from the Bluetooth system.

:Usage: **# unregister-application**

register-service
----------------

Adds a new service under a registered application. This command is crucial for defining new services that devices can offer.

:Usage: **# register-service <UUID> [handle]**

unregister-service
------------------

Removes a service from a registered application, effectively ceasing its availability.

:Usage: **# unregister-service <UUID/object>**

register-includes
-----------------

Marks a service as included within another service, allowing for service hierarchies and complex service structures.

:Usage: **#r egister-includes <UUID> [handle]**

unregister-includes
-------------------

Removes an included service relationship, simplifying the service structure.

:Usage: **# unregister-includes <Service-UUID><Inc-UUID>**

register-characteristic
-----------------------

Introduces a new characteristic under a service, specifying its properties and access permissions with flags.

:Usage: **# register-characteristic <UUID> <Flags=read,write,notify...> [handle]**

unregister-characteristic
-------------------------

Eliminates a characteristic from a service, removing its functionality.

:Usage: **# unregister-characteristic <UUID/object>**

register-descriptor
-------------------

Adds a descriptor to a characteristic, further defining its behavior and access controls.

:Usage: **# register-descriptor <UUID> <Flags=read,write...> [handle]**

unregister-descriptor
---------------------

Removes a descriptor from a characteristic, simplifying its behavior.

:Usage: **# unregister-descriptor <UUID/object>**

RESOURCES
=========

http://www.bluez.org

REPORTING BUGS
==============

linux-bluetooth@vger.kernel.org
