# xcsort

Have you ever looked at **Compiled Sources** and **Copy Bundle Resources** sections under **Build Phase** in **Xcode** and wonder why they are not alphabetically sorted? You might have tried to right click at it and expected it to have *Sort* option which you quickly learn that it's not there. Desperately, you rearrange them manually by dragging them one by one. Then you or your colleagues add more files and they appear randomly in the list. Eventually, you don't give a 💩 anymore.

![Xcode Build Phase](http://i.imgur.com/s6boK2J.png)

This is where **xcsort** come in.

You can use it as one time script or embed it in your project to make it automatically sort your files everytime you build.

## Call from command line
```
curl -LO https://raw.githubusercontent.com/sikhapol/xcsort/master/xcsort
chmod 755 xcsort
./xcsort path/to/Project.xcodeproj/project.pbxproj
```
And that's it.

If you want it to also sort your project structure, uncomment line 44 as shown below:
```
sort_range "children = \(" "\);" 3  | \
```

This will make this:

![Project Structure](http://i.imgur.com/TmAsA4G.png)

become this:

![Project Structure Sorted](http://i.imgur.com/prlywYk.png)

If you want it to do case-insensitive sort, add `-k` to `sort` in line 34
```
done | sort -f -k $3,$3
```

## Embed in Xcode project
If you want your files to be sorted automatically every time you build your project, you can embed it in your project.
In *Build Phase* add *New Run Script Phase* with the [content of the script](https://raw.githubusercontent.com/sikhapol/xcsort/master/xcsort).
Also, put this new run script phase to the very last in the list.
If your *project.pbxproj* file lives somewhere else, change the value of `DEFAULT_PROJECT_FILE_PATH` to that path (line 10).

Some caveat here, when you build your project and there's reordering occurs, the build will be canceled because this script overwrite your *project.pbxproj* which cause the cancelling. But when you build it again and there's no changes, the build will continue.
