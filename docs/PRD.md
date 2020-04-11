# The Simmel project

## Definitions

* **Beacon Ping** or **Ping**: An occasional (once per 10-30 minutes) Bluetooth message indicating presence
* **Device** or **The Device**: A Bluetooth-enabled device that tracks beacon pings
* **Health Authority**: The local agency conducting contact tracing in case of infection
* **User**: The person to whom the Device is registered

## Hardware Description

The _Simmel_ device is physically similar to a USB flash drive with a
keyfob, one green LED and one red LED. The device can be twisted to
disconnect power, during which the device will not function. A
full-sized USB-A connection is present on one side.

When the device is plugged into a PC, it enumerates as an ordinary USB
flash drive. This is used to configure the device.

## Device States

The device exists in three states:

* **Off**: Power is disconnected or the battery is discharged
* **Unconfigured**: The device is brand-new, or has been erased
* **Tracking**: The device will occasionally send beacons and record

When a user first obtains a device, it will be powered off. They must
twist it in order to power it on. This causes it to enter the
**Unconfigured** state. They must use a PC in order to configure it.

## Onboarding Process

When the user plugs the device into a PC, they are presented with an
ordinary USB flash drive. This contains a `.url` file pointing the user
to their local health authority in order to enroll them in the
programme. This URL contains the random token appended to it. The user
will then fill out the form including contact information. Finally, the
website will generate a token file that they must save to the drive.

From this point onwards, the device does not need to be connected to a
PC except to charge it, or in the case where the health authority needs
to perform contact tracing.

## Tracking State

When the Device is tracking, it remains in a low-power state.
Occasionally, it will send a Beacon Ping that includes information that
can be used to do contact tracing. The contents of this Ping vary
depending on the protocol used for tracing.

Additionally, the Device is constantly listening for other Pings. Any
time it receives a Ping, it logs it to an internal journal using the
public key of the Health Authority.

Depending on a configuration flag, the LEDs may or may not flash
whenever a Beacon Ping is generated or received.

## Contact Tracing

When the Health Authority requests contact tracing, they will either ask
the User for the physical token, or if the User is remote they will ask
them to insert the Device into a PC. For verification, the User will be
asked to open a file on the Device with a name such as `challenge.txt` .
This will contain a series of numbers derived from the key of the Health
Authority and a random nonce. This can be used to prove that the
contacting party is the Health Authority in possession of the private
key.

The user may then be asked to type in a code and save the file.

## Data Scrubbing

The data must be scrubbed every two to three weeks, depending on
configuration. This is to ensure privacy, and because contact data from
several weeks back is not epidemiologically useful.

## Configuration

Configuration of the device is done by editing a file on the USB drive.

## Hardware Requirements

The hardware must have the following features

* Bluetooth Low Energy
* Flash storage
* Red LED
* Non-red LED
* USB

## Software Requirements

The software may have the following components:

* Low-power state
* Bluetooth beacon
* Realtime Clock for key rotation
* Encryption libraries
* Configuration text file parser
* USB driver
* USB Mass Storage
* USB DFU for updating

