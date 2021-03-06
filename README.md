Forked from https://github.com/kichik/pecoff4j

# PECOFF4J

PE/COFF 4J is a java engineering library for portable executables, the format used by Windows. It has the following features:

* Parser for Windows executables and DLLs.
* Assembler for creating and modifying executables and DLLs.
* Resource directory parser - understands version info, icons.

This is a fork of http://sourceforge.net/projects/pecoff4j/

Imported from CVS on May 24th, 2014

[![Build Status](https://drone.io/github.com/kichik/pecoff4j/status.png)](https://drone.io/github.com/kichik/pecoff4j/latest)

## Downloads

https://drone.io/github.com/kichik/pecoff4j/files

## License

Sources are licensed under [Common Public License v1.0](http://www.eclipse.org/legal/cpl-v10.html)

## New Features

The project was forked to implement version string parsing for a [StackOverflow question](http://stackoverflow.com/questions/23845480/how-to-get-windows-file-details/23848792).

### Example

```java
import java.io.IOException;

import org.boris.pecoff4j.PE;
import org.boris.pecoff4j.ResourceDirectory;
import org.boris.pecoff4j.ResourceEntry;
import org.boris.pecoff4j.constant.ResourceType;
import org.boris.pecoff4j.io.PEParser;
import org.boris.pecoff4j.io.ResourceParser;
import org.boris.pecoff4j.resources.StringFileInfo;
import org.boris.pecoff4j.resources.StringTable;
import org.boris.pecoff4j.resources.VersionInfo;
import org.boris.pecoff4j.util.ResourceHelper;

public class Main {

	public static void main(String[] args) throws IOException {
		PE pe = PEParser.parse("C:/windows/system32/notepad.exe");
		ResourceDirectory rd = pe.getImageData().getResourceTable();

		ResourceEntry[] entries = ResourceHelper.findResources(rd, ResourceType.VERSION_INFO);
		for (int i = 0; i < entries.length; i++) {
			byte[] data = entries[i].getData();
			VersionInfo version = ResourceParser.readVersionInfo(data);

			StringFileInfo strings = version.getStringFileInfo();
			StringTable table = strings.getTable(0);
			for (int j = 0; j < table.getCount(); j++) {
				String key = table.getString(j).getKey();
				String value = table.getString(j).getValue();
				System.out.println(key + " = " + value);
			}
		}
	}

}

```

Will print:

```
CompanyName = Microsoft Corporation
FileDescription = Notepad
FileVersion = 6.1.7600.16385 (win7_rtm.090713-1255)
InternalName = Notepad
LegalCopyright = © Microsoft Corporation. All rights reserved.
OriginalFilename = NOTEPAD.EXE
ProductName = Microsoft® Windows® Operating System
ProductVersion = 6.1.7600.16385
```

## Other Forks

[jonnyzzz/PE](https://github.com/jonnyzzz/PE) has even more features and probably got much more love than this fork.
