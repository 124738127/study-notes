## 修改键位的Scancode Map介绍

### 修改操作介绍

Scancode Map(扫描码映射)：
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout
中的一个二进制键值(默认没有，需新建)，并且有固定的格式。

Scancode Map 代码的一般格式是：
hex:00,00,00,00,00,00,00,00,|02|,00,00,00,|原键,替代键,原键,替代键,|00,00,00,00

其含义为：
前8个00(DWord两个0)是版本号，接下来的“02”表示映射数，其最小为值为“02”，表示只映射一组，若要映射多组，只需增加相应的值即可，如映射2组其值应为“03”，3组为“04”，4组为“05”

后边代码每4个是一组：前两个是映射后键位的扫描码，后两个是键位原扫描码。如果要交换两个键，则最后四个值的排列形式是：键A，键B，键B，键A——它表示：键A成为键B，键B成为键A

最后以“00,00,00,00” 结尾。

示例：
将CAPSLOCK替换到左边的SHIFT

Windows Registry Editor Version 5.00
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout]
"Scancode Map"=hex:00,00,00,00,00,00,00,00,02,00,00,00,3a,00,2a,00,00,00,00,00

### 我的配置

1. 将右PrtSc替代为Home
2. 将右Ctrl替代为End
3. 将右Alt替代为RMouse

```toml
00 00 00 00 00 00 00 00
04 00 00 00 4F E0 1D E0
5D E0 38 E0 37 E0 47 E0
00 00 00 00 
```

## Scancode映射码

### 基本介绍

scancode集有 set 1、set 2 以及 set 3，一般日常应用中的键盘码集 set 2，以下映射码均基于 set2

参考：[键盘键位修改及管理（Windows篇） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/29581818)

### 小键盘

```toml
"4f 00": "n1"
"50 00": "n2"
"51 00": "n3"
"4b 00": "n4"
"4c 00": "n5"
"4d 00": "n6"
"47 00": "n7"
"48 00": "n8"
"49 00": "n9"
"52 00": "n0"

"53 00": "n."
"4e 00": "n+"
"4a 00": "n-"
"37 00": "n*"
"35 e0": "n/"

"1c e0": "nEnter"

"45 00": "Num Lock"
"46 00": "Scroll Lock"
```

### 主键盘区

```toml
"02 00": "1"
"03 00": "2"
"04 00": "3"
"05 00": "4"
"06 00": "5"
"07 00": "6"
"08 00": "7"
"09 00": "8"
"0a 00": "9"
"0b 00": "0"

"1e 00": "A"
"30 00": "B"
"2e 00": "C"
"20 00": "D"
"12 00": "E"
"21 00": "F"
"22 00": "G"
"23 00": "H"
"17 00": "I"
"24 00": "J"
"25 00": "K"
"26 00": "L"
"32 00": "M"
"31 00": "N"
"18 00": "O"
"19 00": "P"
"10 00": "Q"
"13 00": "R"
"1f 00": "S"
"14 00": "T"
"16 00": "U"
"2f 00": "V"
"11 00": "W"
"2d 00": "X"
"15 00": "Y"
"2c 00": "Z"

"29 00": "`"
"0c 00": "-"
"0d 00": "="

"1a 00": "["
"1b 00": "]"
"2b 00": "\\"

"27 00": ";"
"28 00": "'"

"33 00": ","
"34 00": "."
"35 00": "/"

"0e 00": "Backspace"
"0f 00": "Tab"
"1c 00": "Enter"
"39 00": "Space"
"3a 00": "Caps Lock"
"5d e0": "Application/RMouse"

"2a 00": "Left Shift"
"36 00": "Right Shift"

"1d 00": "Left Ctrl"
"1d e0": "Right Ctrl"

"38 00": "Left Alt"
"38 e0": "Right Alt"

"5b e0": "Left Windows"
"5c e0": "Right Windows"
```

### 扫码区

```toml
"01 00": "Esc"

"3b 00": "F1"
"3c 00": "F2"
"3d 00": "F3"
"3e 00": "F4"
"3f 00": "F5"
"40 00": "F6"
"41 00": "F7"
"42 00": "F8"
"43 00": "F9"
"44 00": "F10"
"57 00": "F11"
"58 00": "F12"
```

### 功能区

```toml
"47 e0": "Home"
"4f e0": "End"
"52 e0": "Insert"
"53 e0": "Delete"
"37 e0": "PrtSc"

"49 e0": "Page Up"
"51 e0": "Page Down"

"4b e0": "Left"
"4d e0": "Right"
"48 e0": "Up"
"50 e0": "Down"
```

### 空

```toml
"00 00": "None"
```



## Scancode映射码_New

[randyrants/sharpkeys: SharpKeys is a utility that manages a Registry key that allows Windows to remap one key to any other key. (github.com)](https://github.com/randyrants/sharpkeys)

```java
            m_hashKeys.Add("00_00", "-- Turn Key Off");
            m_hashKeys.Add("00_01", "Special: Escape");
            m_hashKeys.Add("00_02", "Key: 1 !");
            m_hashKeys.Add("00_03", "Key: 2 @");
            m_hashKeys.Add("00_04", "Key: 3 #");
            m_hashKeys.Add("00_05", "Key: 4 $");
            m_hashKeys.Add("00_06", "Key: 5 %");
            m_hashKeys.Add("00_07", "Key: 6 ^");
            m_hashKeys.Add("00_08", "Key: 7 &");
            m_hashKeys.Add("00_09", "Key: 8 *");
            m_hashKeys.Add("00_0A", "Key: 9 (");
            m_hashKeys.Add("00_0B", "Key: 0 )");
            m_hashKeys.Add("00_0C", "Key: - _");
            m_hashKeys.Add("00_0D", "Key: = +");
            m_hashKeys.Add("00_0E", "Special: Backspace");
            m_hashKeys.Add("00_0F", "Special: Tab");

            m_hashKeys.Add("00_10", "Key: Q");
            m_hashKeys.Add("00_11", "Key: W");
            m_hashKeys.Add("00_12", "Key: E");
            m_hashKeys.Add("00_13", "Key: R");
            m_hashKeys.Add("00_14", "Key: T");
            m_hashKeys.Add("00_15", "Key: Y");
            m_hashKeys.Add("00_16", "Key: U");
            m_hashKeys.Add("00_17", "Key: I");
            m_hashKeys.Add("00_18", "Key: O");
            m_hashKeys.Add("00_19", "Key: P");
            m_hashKeys.Add("00_1A", "Key: [ {");
            m_hashKeys.Add("00_1B", "Key: ] }");
            m_hashKeys.Add("00_1C", "Special: Enter");
            m_hashKeys.Add("00_1D", "Special: Left Ctrl");
            m_hashKeys.Add("00_1E", "Key: A");
            m_hashKeys.Add("00_1F", "Key: S");

            m_hashKeys.Add("00_20", "Key: D");
            m_hashKeys.Add("00_21", "Key: F");
            m_hashKeys.Add("00_22", "Key: G");
            m_hashKeys.Add("00_23", "Key: H");
            m_hashKeys.Add("00_24", "Key: J");
            m_hashKeys.Add("00_25", "Key: K");
            m_hashKeys.Add("00_26", "Key: L");
            m_hashKeys.Add("00_27", "Key: ; :");
            m_hashKeys.Add("00_28", "Key: ' \"");
            m_hashKeys.Add("00_29", "Key: ` ~");
            m_hashKeys.Add("00_2A", "Special: Left Shift");
            m_hashKeys.Add("00_2B", "Key: \\ |");
            m_hashKeys.Add("00_2C", "Key: Z");
            m_hashKeys.Add("00_2D", "Key: X");
            m_hashKeys.Add("00_2E", "Key: C");
            m_hashKeys.Add("00_2F", "Key: V");

            m_hashKeys.Add("00_30", "Key: B");
            m_hashKeys.Add("00_31", "Key: N");
            m_hashKeys.Add("00_32", "Key: M");
            m_hashKeys.Add("00_33", "Key: , <");
            m_hashKeys.Add("00_34", "Key: . >");
            m_hashKeys.Add("00_35", "Key: / ?");
            m_hashKeys.Add("00_36", "Special: Right Shift");
            m_hashKeys.Add("00_37", "Num: *");
            m_hashKeys.Add("00_38", "Special: Left Alt");
            m_hashKeys.Add("00_39", "Special: Space");
            m_hashKeys.Add("00_3A", "Special: Caps Lock");
            m_hashKeys.Add("00_3B", "Function: F1");
            m_hashKeys.Add("00_3C", "Function: F2");
            m_hashKeys.Add("00_3D", "Function: F3");
            m_hashKeys.Add("00_3E", "Function: F4");
            m_hashKeys.Add("00_3F", "Function: F5");

            m_hashKeys.Add("00_40", "Function: F6");
            m_hashKeys.Add("00_41", "Function: F7");
            m_hashKeys.Add("00_42", "Function: F8");
            m_hashKeys.Add("00_43", "Function: F9");
            m_hashKeys.Add("00_44", "Function: F10");
            m_hashKeys.Add("00_45", "Special: Num Lock");
            m_hashKeys.Add("00_46", "Special: Scroll Lock");
            m_hashKeys.Add("00_47", "Num: 7");
            m_hashKeys.Add("00_48", "Num: 8");
            m_hashKeys.Add("00_49", "Num: 9");
            m_hashKeys.Add("00_4A", "Num: -");
            m_hashKeys.Add("00_4B", "Num: 4");
            m_hashKeys.Add("00_4C", "Num: 5");
            m_hashKeys.Add("00_4D", "Num: 6");
            m_hashKeys.Add("00_4E", "Num: +");
            m_hashKeys.Add("00_4F", "Num: 1");

            m_hashKeys.Add("00_50", "Num: 2");
            m_hashKeys.Add("00_51", "Num: 3");
            m_hashKeys.Add("00_52", "Num: 0");
            m_hashKeys.Add("00_53", "Num: .");
            m_hashKeys.Add("00_54", "Unknown: 0x0054");
            m_hashKeys.Add("00_55", "Unknown: 0x0055");
            m_hashKeys.Add("00_56", "Special: ISO extra key");
            m_hashKeys.Add("00_57", "Function: F11");
            m_hashKeys.Add("00_58", "Function: F12");
            m_hashKeys.Add("00_59", "Unknown: 0x0059");
            m_hashKeys.Add("00_5A", "Unknown: 0x005A");
            m_hashKeys.Add("00_5B", "Unknown: 0x005B");
            m_hashKeys.Add("00_5C", "Unknown: 0x005C");
            m_hashKeys.Add("00_5D", "Unknown: 0x005D");
            m_hashKeys.Add("00_5E", "Unknown: 0x005E");
            m_hashKeys.Add("00_5F", "Unknown: 0x005F");

            m_hashKeys.Add("00_60", "Unknown: 0x0060");
            m_hashKeys.Add("00_61", "Unknown: 0x0061");
            m_hashKeys.Add("00_62", "Unknown: 0x0062");
            m_hashKeys.Add("00_63", "Unknown: 0x0063");
            m_hashKeys.Add("00_64", "Function: F13");
            m_hashKeys.Add("00_65", "Function: F14");
            m_hashKeys.Add("00_66", "Function: F15");
            m_hashKeys.Add("00_67", "Function: F16");   // Mac keyboard 
            m_hashKeys.Add("00_68", "Function: F17");   // Mac keyboard
            m_hashKeys.Add("00_69", "Function: F18");   // Mac keyboard
            m_hashKeys.Add("00_6A", "Function: F19");   // Mac keyboard
            m_hashKeys.Add("00_6B", "Function: F20");   // IBM Model F 122-keys
            m_hashKeys.Add("00_6C", "Function: F21");   // IBM Model F 122-keys
            m_hashKeys.Add("00_6D", "Function: F22");   // IBM Model F 122-keys
            m_hashKeys.Add("00_6E", "Function: F23");   // IBM Model F 122-keys
            m_hashKeys.Add("00_6F", "Function: F24");   // IBM Model F 122-keys

            m_hashKeys.Add("00_70", "Unknown: 0x0070");
            m_hashKeys.Add("00_71", "Unknown: 0x0071");
            m_hashKeys.Add("00_72", "Unknown: 0x0072");
            m_hashKeys.Add("00_73", "Unknown: 0x0073");
            m_hashKeys.Add("00_74", "Unknown: 0x0074");
            m_hashKeys.Add("00_75", "Unknown: 0x0075");
            m_hashKeys.Add("00_76", "Unknown: 0x0076");
            m_hashKeys.Add("00_77", "Unknown: 0x0077");
            m_hashKeys.Add("00_78", "Unknown: 0x0078");
            m_hashKeys.Add("00_79", "Special: Henkan");
            m_hashKeys.Add("00_7A", "Unknown: 0x007A");
            m_hashKeys.Add("00_7B", "Special: Muhenkan");
            m_hashKeys.Add("00_7C", "Unknown: 0x007C");
            m_hashKeys.Add("00_7D", "Special: ¥ -");
            m_hashKeys.Add("00_7E", "Unknown: 0x007E");
            m_hashKeys.Add("00_7F", "Unknown: 0x007F");

            m_hashKeys.Add("E0_01", "Unknown: 0xE001");
            m_hashKeys.Add("E0_02", "Unknown: 0xE002");
            m_hashKeys.Add("E0_03", "Unknown: 0xE003");
            m_hashKeys.Add("E0_04", "Unknown: 0xE004");
            m_hashKeys.Add("E0_05", "Unknown: 0xE005");
            m_hashKeys.Add("E0_06", "Unknown: 0xE006");
            m_hashKeys.Add("E0_07", "F-Lock: Redo");
            m_hashKeys.Add("E0_08", "F-Lock: Undo");
            m_hashKeys.Add("E0_09", "Unknown: 0xE009");
            m_hashKeys.Add("E0_0A", "Unknown: 0xE00A");
            m_hashKeys.Add("E0_0B", "Unknown: 0xE00B");
            m_hashKeys.Add("E0_0C", "Unknown: 0xE00C");
            m_hashKeys.Add("E0_0D", "Unknown: 0xE00D");
            m_hashKeys.Add("E0_0E", "Unknown: 0xE00E");
            m_hashKeys.Add("E0_0F", "Unknown: 0xE00F");

            m_hashKeys.Add("E0_10", "Media: Prev Track");
            m_hashKeys.Add("E0_11", "App: Messenger");
            m_hashKeys.Add("E0_12", "Logitech: Webcam");
            m_hashKeys.Add("E0_13", "Logitech: iTouch");
            m_hashKeys.Add("E0_14", "Logitech: Shopping");
            m_hashKeys.Add("E0_15", "Unknown: 0xE015");
            m_hashKeys.Add("E0_16", "Unknown: 0xE016");
            m_hashKeys.Add("E0_17", "Unknown: 0xE017");
            m_hashKeys.Add("E0_18", "Unknown: 0xE018");
            m_hashKeys.Add("E0_19", "Media: Next Track");
            m_hashKeys.Add("E0_1A", "Unknown: 0xE01A");
            m_hashKeys.Add("E0_1B", "Unknown: 0xE01B");
            m_hashKeys.Add("E0_1C", "Num: Enter");
            m_hashKeys.Add("E0_1D", "Special: Right Ctrl");
            m_hashKeys.Add("E0_1E", "Unknown: 0xE01E");
            m_hashKeys.Add("E0_1F", "Unknown: 0xE01F");

            m_hashKeys.Add("E0_20", "Media: Mute");
            m_hashKeys.Add("E0_2038", "Special: Alt Gr");
            m_hashKeys.Add("E0_21", "App: Calculator");
            m_hashKeys.Add("E0_22", "Media: Play/Pause");
            m_hashKeys.Add("E0_23", "F-Lock: Spell");
            m_hashKeys.Add("E0_24", "Media: Stop");
            m_hashKeys.Add("E0_25", "Unknown: 0xE025");
            m_hashKeys.Add("E0_26", "Unknown: 0xE026");
            m_hashKeys.Add("E0_27", "Unknown: 0xE027");
            m_hashKeys.Add("E0_28", "Unknown: 0xE028");
            m_hashKeys.Add("E0_29", "Unknown: 0xE029");
            m_hashKeys.Add("E0_2A", "Unknown: 0xE02A");
            m_hashKeys.Add("E0_2B", "Unknown: 0xE02B");
            m_hashKeys.Add("E0_2C", "Unknown: 0xE02C");
            m_hashKeys.Add("E0_2D", "Unknown: 0xE02D");
            m_hashKeys.Add("E0_2E", "Media: Volume Down");
            m_hashKeys.Add("E0_2F", "Unknown: 0xE02F");

            m_hashKeys.Add("E0_30", "Media: Volume Up");
            m_hashKeys.Add("E0_31", "Unknown: 0xE031");
            m_hashKeys.Add("E0_32", "Web: Home");
            m_hashKeys.Add("E0_33", "Unknown: 0xE033");
            m_hashKeys.Add("E0_34", "Unknown: 0xE034");
            m_hashKeys.Add("E0_35", "Num: /");
            m_hashKeys.Add("E0_36", "Unknown: 0xE036");
            m_hashKeys.Add("E0_37", "Special: PrtSc");
            m_hashKeys.Add("E0_38", "Special: Right Alt");
            m_hashKeys.Add("E0_39", "Unknown: 0xE039");
            m_hashKeys.Add("E0_3A", "Unknown: 0xE03A");
            m_hashKeys.Add("E0_3B", "F-Lock: Help");
            m_hashKeys.Add("E0_3C", "F-Lock: Office Home");
            m_hashKeys.Add("E0_3D", "F-Lock: Task Pane");
            m_hashKeys.Add("E0_3E", "F-Lock: New");
            m_hashKeys.Add("E0_3F", "F-Lock: Open");

            m_hashKeys.Add("E0_40", "F-Lock: Close");
            m_hashKeys.Add("E0_41", "F-Lock: Reply");
            m_hashKeys.Add("E0_42", "F-Lock: Fwd");
            m_hashKeys.Add("E0_43", "F-Lock: Send");
            m_hashKeys.Add("E0_44", "Unknown: 0xE044");
            m_hashKeys.Add("E0_45", "Special: €");
            m_hashKeys.Add("E0_46", "Special: Break");
            m_hashKeys.Add("E0_47", "Special: Home");
            m_hashKeys.Add("E0_48", "Arrow: Up");
            m_hashKeys.Add("E0_49", "Special: Page Up");
            m_hashKeys.Add("E0_4A", "Unknown: 0xE04A");
            m_hashKeys.Add("E0_4B", "Arrow: Left");
            m_hashKeys.Add("E0_4C", "Unknown: 0xE04C");
            m_hashKeys.Add("E0_4D", "Arrow: Right");
            m_hashKeys.Add("E0_4E", "Unknown: 0xE04E");
            m_hashKeys.Add("E0_4F", "Special: End");

            m_hashKeys.Add("E0_50", "Arrow: Down");
            m_hashKeys.Add("E0_51", "Special: Page Down");
            m_hashKeys.Add("E0_52", "Special: Insert");
            m_hashKeys.Add("E0_53", "Special: Delete");
            m_hashKeys.Add("E0_54", "Unknown: 0xE054");
            m_hashKeys.Add("E0_55", "Unknown: 0xE055");
            m_hashKeys.Add("E0_56", "Special: < > |");
            m_hashKeys.Add("E0_57", "F-Lock: Save");
            m_hashKeys.Add("E0_58", "F-Lock: Print");
            m_hashKeys.Add("E0_59", "Unknown: 0xE059");
            m_hashKeys.Add("E0_5A", "Unknown: 0xE05A");
            m_hashKeys.Add("E0_5B", "Special: Left Windows");
            m_hashKeys.Add("E0_5C", "Special: Right Windows");
            m_hashKeys.Add("E0_5D", "Special: Application");
            m_hashKeys.Add("E0_5E", "Special: Power");
            m_hashKeys.Add("E0_5F", "Special: Sleep");

            m_hashKeys.Add("E0_60", "Unknown: 0xE060");
            m_hashKeys.Add("E0_61", "Unknown: 0xE061");
            m_hashKeys.Add("E0_62", "Unknown: 0xE062");
            m_hashKeys.Add("E0_63", "Special: Wake (or Fn)");
            m_hashKeys.Add("E0_64", "Unknown: 0xE064");
            m_hashKeys.Add("E0_65", "Web: Search");
            m_hashKeys.Add("E0_66", "Web: Favorites");
            m_hashKeys.Add("E0_67", "Web: Refresh");
            m_hashKeys.Add("E0_68", "Web: Stop");
            m_hashKeys.Add("E0_69", "Web: Forward");
            m_hashKeys.Add("E0_6A", "Web: Back");
            m_hashKeys.Add("E0_6B", "App: My Computer");
            m_hashKeys.Add("E0_6C", "App: Mail");
            m_hashKeys.Add("E0_6D", "App: Media Select");
            m_hashKeys.Add("E0_6E", "Unknown: 0xE06E");
            m_hashKeys.Add("E0_6F", "Unknown: 0xE06F");

            m_hashKeys.Add("E0_70", "Unknown: 0xE070");
            m_hashKeys.Add("E0_71", "Unknown: 0xE071");
            m_hashKeys.Add("E0_72", "Unknown: 0xE072");
            m_hashKeys.Add("E0_73", "Unknown: 0xE073");
            m_hashKeys.Add("E0_74", "Unknown: 0xE074");
            m_hashKeys.Add("E0_75", "Unknown: 0xE075");
            m_hashKeys.Add("E0_76", "Unknown: 0xE076");
            m_hashKeys.Add("E0_77", "Unknown: 0xE077");
            m_hashKeys.Add("E0_78", "Unknown: 0xE078");
            m_hashKeys.Add("E0_79", "Unknown: 0xE079");
            m_hashKeys.Add("E0_7A", "Unknown: 0xE07A");
            m_hashKeys.Add("E0_7B", "Unknown: 0xE07B");
            m_hashKeys.Add("E0_7C", "Unknown: 0xE07C");
            m_hashKeys.Add("E0_7D", "Unknown: 0xE07D");
            m_hashKeys.Add("E0_7E", "Unknown: 0xE07E");
            m_hashKeys.Add("E0_7F", "Unknown: 0xE07F");

            m_hashKeys.Add("E0_A4", "Unknown: 0xE0A4"); // Possibly Left MENU key
            m_hashKeys.Add("E0_A5", "Unknown: 0xE0A5"); // Possibly Right MENU key
            m_hashKeys.Add("E0_A6", "Unknown: 0xE0A6"); // Possibly Browser Back key
            m_hashKeys.Add("E0_A7", "Unknown: 0xE0A7"); // Possibly Browser Forward key
            m_hashKeys.Add("E0_A8", "Unknown: 0xE0A8"); // Possibly Browser Refresh key
            m_hashKeys.Add("E0_A9", "Unknown: 0xE0A9"); // Possibly Browser Stop key
            m_hashKeys.Add("E0_AA", "Unknown: 0xE0AA"); // Possibly Browser Search key
            m_hashKeys.Add("E0_AB", "Unknown: 0xE0AB"); // Possibly Browser Favorites key
            m_hashKeys.Add("E0_AC", "Unknown: 0xE0AC"); // Possibly Browser Start and Home key
            m_hashKeys.Add("E0_AD", "Unknown: 0xE0AD"); // Possibly Volume Mute key
            m_hashKeys.Add("E0_AE", "Unknown: 0xE0AE"); // Possibly Volume Down key
            m_hashKeys.Add("E0_AF", "Unknown: 0xE0AF"); // Possibly Volume Up key

            m_hashKeys.Add("E0_B0", "Unknown: 0xE0B0"); // Media: Next track (alternate)
            m_hashKeys.Add("E0_B1", "Unknown: 0xE0B1"); // Media: Previous track (alternate)
            m_hashKeys.Add("E0_B2", "Unknown: 0xE0B2"); // Media: Stop (alternate)
            m_hashKeys.Add("E0_B3", "Unknown: 0xE0B3"); // Media: Play/Pause (alternate)
            m_hashKeys.Add("E0_B4", "Unknown: 0xE0B4"); // App: Mail (alternate)
            m_hashKeys.Add("E0_B5", "Unknown: 0xE0B5"); // App: Select Media key
            m_hashKeys.Add("E0_B6", "Unknown: 0xE0B6"); // Start Application 1 key
            m_hashKeys.Add("E0_B7", "Unknown: 0xE0B7"); // Start Application 2 key
            m_hashKeys.Add("E0_B8", "Unknown: 0xE0B8"); // Reserved
            m_hashKeys.Add("E0_B9", "Unknown: 0xE0B9"); // Reserved
            m_hashKeys.Add("E0_BA", "Unknown: 0xE0BA"); // Used for miscellaneous characters; it can vary by keyboard.
            m_hashKeys.Add("E0_BB", "Unknown: 0xE0BB"); // For any country/region, the '+' key
            m_hashKeys.Add("E0_BC", "Unknown: 0xE0BC"); // For any country/region, the ',' key
            m_hashKeys.Add("E0_BD", "Unknown: 0xE0BD"); // For any country/region, the '-' key
            m_hashKeys.Add("E0_BE", "Unknown: 0xE0BE"); // For any country/region, the '.' key
            m_hashKeys.Add("E0_BF", "Unknown: 0xE0BF"); // Varies by keyboard

            m_hashKeys.Add("E0_C0", "Unknown: 0xE0C0"); // Unknown key

            m_hashKeys.Add("E0_DB", "Unknown: 0xE0BB"); // Varies by keyboard
            m_hashKeys.Add("E0_DC", "Unknown: 0xE0BC"); // Varies by keyboard
            m_hashKeys.Add("E0_DD", "Unknown: 0xE0BD"); // Varies by keyboard
            m_hashKeys.Add("E0_DE", "Unknown: 0xE0BE"); // Varies by keyboard
            m_hashKeys.Add("E0_DF", "Unknown: 0xE0BF"); // Varies by keyboard

            m_hashKeys.Add("E0_E1", "Unknown: 0xE0B1"); // Varies by keyboard
            m_hashKeys.Add("E0_E2", "Unknown: 0xE0B2"); // Varies by keyboard
            m_hashKeys.Add("E0_E3", "Unknown: 0xE0B3"); // Varies by keyboard
            m_hashKeys.Add("E0_E4", "Unknown: 0xE0B4"); // Varies by keyboard
                                                        
            m_hashKeys.Add("E0_F1", "Special: Hanja Key");
            m_hashKeys.Add("E0_F2", "Special: Hangul Key");
```






