# MePOS Connect SDK iOS user guide V1.0

The MePOS connect iOS SDK is designed to allow communication from any iOS 9 or iOS 10 capable device.
This document is a reference on how to integrate to the MePOS unit into your own tablet application. This document does
not include information on how to set up the MePOS unit, for this please refer to the documentation that came with your
MePOS unit.

## Contents
- [Supported tablet devices](#supported-tablet-devices)
- [Supported MePOS devices](#supported-mepos-devices)
- [Use of the MePOS connect SDK on iOS](#use-of-the-mepos-connect-sdk-on-ios)
- [Version](#version)
- [Libraries](#libraries)
- [Creating a new MePOS object](#creating-a-new-mepos-object)
- [MePOS SDK Methods](#mepos-sdk-methods)
- [int enableUSB()](#jintenableusb)
- [int disableUSB()](#jintdisableusb)
- [int enableWifi()](#voidenablewifi)
- [int disableWifi()](#voiddisablewifi)
- [int printRAW(String command)](#jintprintrawwithnsstringnsstring--rawdata)
- [printerBusy()](#jintprinterbusy)
- [MePOSConnectionManager getConnectionManager()](#comuniquesecuremeposconnectmeposconnectionmanagergetconnectionmanager)
- [Open cashdrawer](#voidopencashdrawer)
- [Print config page](#voidprintconfigpage)
- [int getConnectionStatus()](#jintgetconnectionstatus)
- [setConnectionIPAddress(string IPAddress)](#voidsetconnectionipaddresswithnsstringnsstringipaddress)
- [String getConnectionIPAddress()](#nsstringgetconnectionipaddress)
- [boolean MePOSConnectWiFi(String SSID, String IPAddress, String netmask, String encryption, String password)]()
- [String MePOSGetAssignedIP()](#nsstringmeposgetassignedip)
- [boolean MePOSSetAccessPoint(String SSID, String encryption, String password)]()
- [MePOSReceipt](#comuniquesecuremeposconnectmeposreceipt)
- [setCutType(int cutType)](#voidsetcuttypewithintjintcuttype)
- [MePOSReceiptBarcodeLine(int type, String data)](#comuniquesecuremeposconnectmeposreceiptbarcodelinejinttype-withnsstringnsstringdata)
- [MePOSReceiptFeedLine(int lines)](#comuniquesecuremeposconnectmeposreceiptfeedlinejintlines)
- [MePOSReceiptImageLine(Bitmap image)](#comuniquesecuremeposconnectmeposreceiptsinglecharlinejcharchr)
- [Text style constants](#text-style-constants)
- [Text size constants](#text-size-constants)
- [Text position constants](#text-position-constants)

## Supported tablet devices
The MePOS connect library supports Android tablets and Windows PC’s via a USB and Wi-Fi connection. Due to the
restrictions of the iOS platform it is not possible to connect to the MePOS unit via USB from an Apple device.

## Supported MePOS devices

The MePOS connect SDK has been tested with the latest MePOS 2.8 firmware.

## Use of the MePOS connect SDK on IOs

## Version
The current iOS library version is 1.0.0, details of the latest changes are in the release notes bundled with the iOS
SDK.

## Frameworks
In the distribution zip file with the iOS SDK there is a frameworks folder. The 8 frameworks in this folder must
be added to your Xcode project.

This must be done in two places:

1) Add the frameworks to your project (Build Phases -> Link Binary -> + -> Other).

2) Add the path to the frameworks to the library search paths (Build Settings->Search Paths->Library Search
Paths).

## Includes
The header files for the MePOS connect library must be referenced by your project. In the distribution zip file with the iOS SDK there is an include folder. Add these to your project (Build Settings->Search Paths->Header Search Paths).

## Libraries
In the distribution zip file with the iOS SDK there are two libraries. One is for the simulator and one for iOS devices. You will need to add the relevant one to your project (Build Phases->Link Binary-> + -> Other).

## Creating a new MePOS object
To create a new mepos object in your application you can use the following code in the viewDidLoad section of your code.

```
(void)viewDidLoad {
  [super viewDidLoad];
m = [ComUniquesecureMeposconnectMePOS new];
}
```

##MePOS SDK Methods

Once a MePOS object has been created there are several methods that can be executed that perform actions on the MePOS unit. The method names, syntax and usage are identical across the Windows and Android platform.

The MePOS methods will return 0 if successful or 1 there is no connection to the MePOS, it is possible to query
the connection state of the MePOS using the MePOSConnectionManager before a command is sent. During the
initialisation of the MePOSConnectionManager the status may briefly read -1.

**setLedOneColWithJavaLangInteger:(JavaLangInteger*)[NSNumber numberWithInt:int colour]**

**setLedTwoColWithJavaLangInteger:(JavaLangInteger*)[NSNumber numberWithInt:int colour]**

**setLedTwoColWithJavaLangInteger:(JavaLangInteger*)[NSNumber numberWithInt:int colour]**

Will set one of the three diagnostic LED's on the MePOS unit to one of the following colours:

0 = Black(off)

1 = Green

2 = Red

3 = Amber

**Note:** Between the development version and early versions of the MePOS unit the colours 1 and 2 (Green and Red are swapped).

**(jint)setCosmeticLedColWithJavaLangInteger:(JavaLangInteger*)[NSNumber numberWithInt:int colour]**
Will set the MePOS cosmetic LED to one of the following colours:

- 0 = Black(off)
- 1 = Blue
- 2 = Green
- 3 = Cyan
- 4 = Red
- 5 = Magenta
- 6 = Yellow
- 7 = White

**(jint)printWithComUniquesecureMeposconnectMePOSReceipt:ComUniquesecureMeposconnectMePOSReceipt**

Prints a pre-defined MePOS receipt using the built in receipt printer. To print a receipt, you must first create a MePOS receipt and add lines to it using the add command. This method will return 0 for success, 1 if no MePOS is connected or 2 if the printer is busy.

The below example prints a single line receipt:

```
ComUniquesecureMeposconnectMePOSReceipt *r = [ComUniquesecureMeposconnectMePOSReceipt new];
 ComUniquesecureMeposconnectMePOSReceiptTextLine *l = [ComUniquesecureMeposconnectMePOSReceiptTextLine new];
[l setTextWithNSString:@"TEST" withInt: 1 withInt: 1 withInt: 1];
[r addLineWithComUniquesecureMeposconnectMePOSReceiptLine:l];

 [m printWithComUniquesecureMeposconnectMePOSReceipt:r]
```

## (jint)enableUSB

Enables the USB ports on the MePOS device.

## (jint)disableUSB

Disables the USB ports on the MePOS device.

## (void)enableWifi

Enables the Wifi module on the MePOS device.

## (void)disableWifi
Disables the Wifi module on the MePOS device.
## (jint)printRAWWithNSString:(NSString * )rawData

The raw print command allows the user to send ESC POS commands directly to the printer without using the receipt builder. This function is useful if your epos system already prints using the ESC POS command set. This method will return 0 for success, 1 if no MePOS is connected or 2 if the printer is busy.

## (jint)printerBusy

Print commands are sent to the printer asynchronously, print or printRAW will return immediately to prevent locking the UI thread on a tablet device. To control the tablet UI and prevent possible print buffer overflow it is possible to monitor the printer busy status. New receipts cannot not be printed unless the printerBusy method returns false.

## (ComUniquesecureMeposconnectMePOSConnectionManager*)getConnectionManager

Gets the MePOSConnectionManager for the current MePOS instance. The connection manager can be used to set up the Wi-Fi network on the MePOS unit and query the connection state of the MePOS unit.

## (void)OpenCashDrawer
Opens the MePOS cash drawer.

## (void)PrintConfigPage

Prints a printer config page.

## ComUniquesecureMeposconnectMePOSConnectionManager

The MePOSConnectionManager can be used to configure the connection settings for the MePOS unit and to configure the Wi-Fi module on a MePOS unit.

## (jint)getConnectionStatus

Gets the current connection state of the MePOS unit:

- -1 = Not initialised
- 0 = Not connected
- 1 = Connected via USB (Not valid for iOS)
- 2 = Connected via Wi-Fi

**Note:** The USB connection to the MePOS unit will always take priority over Wi-Fi, to test the Wi-Fi connectivity the tablet must be disconnected from USB.

## (void)setConnectionIPAddressWithNSString:(NSString*)IPAddress

Sets the IP address on which the connection manager will look for a MePOS unit. The default IP address of the MePOS from the factory is 192.168.16.254, if the tablet is connecting to the MePOS as a client then you will not need to change this parameter unless the network settings on the MePOS unit have been changed.

## (NSString*)getConnectionIPAddress;

Gets the current IP address setting for the connection manager.
```
(jboolean)MePOSConnectWiFiWithNSString:(NSString *)SSID
 withNSString:(NSString *)IPAddress
 withNSString:(NSString *)netmask
 withNSString:(NSString *)encryption
 withNSString:(NSString *)password
 ```
Connects the MePOS unit to a WiFi network as a client. After performing a Wi-Fi connection the setConnectionIPAddress method must be called with the provided static IP address or the assigned DHCP IP address using the MePOSGetAssignedIP method. If the MePOS unit is being used as an access point, connecting
to a Wi-Fi network will switch the MePOS to becoming a Wi-Fi client and the MePOS unit will no longer be a Wi-Fi access point. It is only possible to configure the WiFi module when the MePOS unit is plugged in via USB, a call to this method will return false if it is no USB connection is found.

Valid input parameters are:
SSID – The SSID of the Wi-Fi network you are connecting to.

IPAddress – A valid static IP Address e.g. 192.168.0.100 or DHCP to request a DHCP address from the network.

Netmask – A valid network mask e.g. 255.255.255.0, if the IPAddress parameter was DHCP this parameter will be ignored.

Encryption – The encryption type of the network you are connecting to, one of the following values NONE, WEP, WEP_OPEN, WPA_TKIP, WPA_AES, WPA2_TKIP, WPA2_AES, WPAWPA2_TKIP, WPAWPA2_AES

Password - The password for the network you are connecting to. This can be left blank or will be ignored if the
encryption type was set to NONE.

## (NSString*)MePOSGetAssignedIP

Gets the current IP address from the connected MePOS unit. When connected to Wi-Fi this will be either the statically provided IP address or the DHCP network assigned IP address. In access point mode this will be the default IP address of 192.168.16.254.
```
(jboolean)MePOSSetAccessPointWithNSString:(NSString *)SSID

 withNSString:(NSString *)encryption
 withNSString:(NSString *)password
 ```

Sets the MePOS unit in to access point mode. When entering access point mode, the MePOS unit will create its own Wi-Fi network with the SSID, encryption and password provided. In access point mode the MePOS unit will create the IP network 192.168.16.0 and will get the IP address 192.168.16.254. Clients connecting to the
MePOS unit will be automatically assigned IP addresses via DHCP. If the MePOS unit is connected to a Wi-Fi network as a client, setting the MePOS unit in access point mode will remove the MePOS unit from any Wi-Fi networks it is connected to in client mode. It is only possible to configure the WiFi module when the MePOS
unit is plugged in via USB, a call to this method will return false if it is no USB connection is found.

SSID – The SSID of the Wi-Fi network you are creating.
Encryption – The encryption type of the network you are creating, one of the following values NONE, WEP, WEP_OPEN, WPA_TKIP, WPA_AES, WPA2_TKIP, WPA2_AES, WPAWPA2_TKIP, WPAWPA2_AES
Password - The password for the network you are creating. This can be left blank or will be ignored if the encryption type was set to NONE.

## ComUniquesecureMeposconnectMePOSReceipt
The MePOS library also contains the classes to define and print a receipt using the receipt printer.

To create a new receipt, use the following code:

```
ComUniquesecureMeposconnectMePOSReceipt *r = [ComUniquesecureMeposconnectMePOSReceipt new];
```

After the receipt has been initialised you can add lines to the receipt for printing. There are currently six types of line available to print on a receipt, these are detailed below.

## (void)setCutTypeWithInt:(jint)cutType
Specifies whether to perform a full or partial cut at the end of the receipt where the cut type is:

- MePOS.CUT_TYPE_FULL
- MePOS.CUT_TYPE_PARTIAL

Once the receipt has been created you can modify how the printer will cut the receipt after it has finished printing. As a default the printer is set to perform a full receipt cut.

## ComUniquesecureMeposconnectMePOSReceiptBarcodeLine(jint)type withNSString:(NSString*)data;

The barcode line can be used to add a barcode to a receipt. There are currently three supported barcode types,
UPC-A, Code 39 and PDF417. They are specified using the following constants:
- MePOS.BARCODE_TYPE_UPCA
- MePOS.BARCODE_TYPE_CODE39
- MePOS.BARCODE_TYPE_PDF417


## ComUniquesecureMeposconnectMePOSReceiptFeedLine(jint)lines

The feed line can be used to add whitespace to a receipt. The parameter supplied is the number of lines to feed.

```
ComUniquesecureMeposconnectMePOSReceiptPriceLine (NSString *)leftText

withInt:(jint)leftStyle
 withNSString:(NSString *)rightText
 withInt:(jint)rightStyle
 ```

The price line can be used to add a line to a receipt with text on the left and right simulating the common price item layout of many receipts. The price line takes parameters defining the text on the left and right and also the style of the text. Text styles are discussed later in this document.

## ComUniquesecureMeposconnectMePOSReceiptSingleCharLine(jchar)chr
The single character line can be used to fill a single line with the same character. The parameter is the character to repeat across the whole line.
```
ComUniquesecureMeposconnectMePOSReceiptTextLine (NSString *)text
 withInt:(jint)style
 withInt:(jint)size
 withInt:(jint)position
 ```

The text line can be used to put text on a receipt on the left centre or right in different sizes or styles. The first parameter is the text to print, the size and position constants are discussed later in this document.

## Text style constants

Some of the printer line commands accept a style parameter. This parameter can be any one of the following

constants:
- MePOS.TEXT_STYLE_NONE
- MePOS.TEXT_STYLE_BOLD
- MePOS.TEXT_STYLE_ITALIC
- MePOS.TEXT_STYLE_UNDERLINED
- MePOS.TEXT_STYLE_INVERSE

Text styles can also be combined using the or operator to achieve a mix of styles, for example to print bold italic text you would use the following:

- MePOS.TEXT_STYLE_BOLD | MePOS.TEXT_STYLE_ITALIC

## Text size constants
Some of the printer line commands accept a size constant. This can be one of the following but not both:

- MePOS.TEXT_SIZE_NORMAL
- MePOS.TEXT_SIZE_WIDE

## Text position constants
Some of the printer line commands accept a position constant. This can be any of the following:

- MePOS.TEXT_POSITION_LEFT
- MePOS.TEXT_POSITION_CENTER
- MePOS.TEXT_POSITION_RIGHT
