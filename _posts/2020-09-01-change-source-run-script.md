---
title: "Changing resources for different environments using Run Scripts"
last_modified_at: 2020-09-01T19:20:02-05:00
categories:
  - Blog
tags:
  - Swift
  - Infoplist.strings
  - Localization
  - RunScripts
---

# Changing InfoPlist.strings for different environments

if you have multiple environments for your project such as TEST and PROD in different targets with different app name, and if your localized InfoPlist.strings file contains that app name , you need to change that InfoPlist.strings files content for every language . you can make a copy of that strings file and exclude for one target but thing get messy with folder structures.
You can also make dynamic app name with your Localizable.strings.
in any localization file if you have app name like:

```strings
app-photo-access = "%@ needs photo access";
```

 you can use String format like this:

```swift
String(format: NSLocalizedString("app-photo-access", comment: ""), Bundle.appName())

extension Bundle {
    static func appName() -> String {
        guard let dictionary = Bundle.main.infoDictionary else {
            return ""
        }
        if let version : String = dictionary["CFBundleName"] as? String {
            return version
        } else {
            return ""
        }
    }
}
```

but this method is not applicable for InfoPlist.strings file.
As one of the solutions: we can use Run Scripts. Here is how to do that with run scripts:

```shell
for i in $(find . -name "InfoPlist.strings" ); do
sed -i '' -e 's/TEST_APP_NAME/PROD_APP_NAME/g' $i
done
```

1. this is the run script that we will use. Lets break it down.
we will look all of `InfoPlist.string` file in our project folder using shell iterate all the files that is under *.lproj folder.
2. `sed` is stream editor. According to man page of sed command:

>The sed utility reads the specified files, or the standard input if no files are specified, modifying the input as specified by a list of commands. The input is then written to the standard output.

using sed we will be able to read of the content of strings file and modify its content.
-i is for:
>Edit files in-place, saving backups with the specified extension. If a zero-length extension is given, no backup will be saved.

-e is for:
>Append the editing commands specified by the command argument to the list of commands.

