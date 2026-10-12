.. _nrf_desktop_ble_adv:

Bluetooth LE advertising module
###############################

.. contents::
   :local:
   :depth: 2

In nRF Desktop, the |ble_adv| is responsible for controlling the Bluetooth® LE advertising for the nRF Desktop peripheral device.

Module events
*************

.. include:: event_propagation.rst
    :start-after: table_ble_adv_start
    :end-before: table_ble_adv_end

.. note::
    |nrf_desktop_module_event_note|

Configuration
*************

nRF Desktop uses an application-specific |ble_adv| implementation (:file:`src/modules/ble_adv.c`).
The implementation is ported from the :ref:`CAF Bluetooth LE advertising <nrf:caf_ble_adv>` module.
To enable the module, set the :option:`CONFIG_DESKTOP_BLE_ADV` Kconfig option to ``y``.
Module options use the ``CONFIG_DESKTOP_BLE_ADV_*`` naming scheme (for example, :option:`CONFIG_DESKTOP_BLE_ADV_FILTER_ACCEPT_LIST`).
For details on the default configuration alignment, see the following sections.

For more information about Bluetooth configuration in nRF Desktop, see :ref:`nrf_desktop_bluetooth_guide`.

Avoiding connection requests from unbonded centrals when bonded
===============================================================

If the Bluetooth local identity currently in use already has a bond and the nRF Desktop device uses indirect advertising, the device will not set the General Discoverable flag.
The nRF Desktop devices also enable :option:`CONFIG_DESKTOP_BLE_ADV_FILTER_ACCEPT_LIST` to filter incoming scan response data requests and connection requests.
This is done to prevent Bluetooth Centrals other than the bonded one from connecting with the device.
The nRF Desktop dongle scans for peripheral devices using the Bluetooth device name, which is provided in the scan response data.

Advertised data
===============

The |ble_adv| relies on :ref:`nrf:bt_le_adv_prov_readme` to manage advertising data and scan response data.
The nRF Desktop application configures the data providers in the :file:`src/modules/Kconfig.ble_adv.default` file.
By default, the application enables a set of data providers available in the |NCS| and adds a custom provider of UUID16 values of Battery Service (BAS) and Human Interface Device Service (HIDS).
The UUID16 of a given GATT Service is added to the advertising data only if the service is enabled in the configuration and the Bluetooth local identity in use has no bond.

Implementation details
**********************

Currently, the behavior of the nRF Desktop |ble_adv| matches the behavior of the CAF |ble_adv|.
See the :ref:`CAF Bluetooth LE advertising <nrf:caf_ble_adv>` page for implementation details.
