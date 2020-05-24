A friend of mine was inspired to start playing computer games during her COVID-19 quarantine. She has a mac and the game she planned to play was on Steam so a quick download of a standard `.dmg` was in order. She was then greeted with an unfriendly message when she opened the app for the first time.

> Steam requires that '/Users/\<username\>/Library/Application Support/Steam/Steam.AppBundle/Steam/Contents/MacOS' be on a case-insensitive filesystem.

She's wasn't sure exactly what that meant and so she reached out to me for help.

I was able to find a Steam support post[^1] that said case-sensitive filesystems aren't supported. That's annoying because moving back to a case-insensitive filesystem is not an easy fix. It requires backing up all data onto another drive or partition and then reformating the case-sensitive drive/partition to be case-insensitive. Maybe I'll help her with this transition later but for now this isn't an option.

Also from the same post:

> For advanced users familiar with Unix and Terminal, you may be able to get your case-sensitive system to work by using symbolic links.

That gave me some hope so I decided to give it a try.

The first step was to make a new case-insensitive partition. There's a lot of info[^2] already around the internet on doing this but please be careful as a mistake could cause significant data loss.

Next, I made a folder in the root of the new partition (the name isn't important here) and an alias named `Steam` from within Finder. I decided to use a macOS alias because I naively assumed that they were more flexible. Then I moved this alias to `~/Library/Application Support/` (after removing the folder of the same name if needed). I then proceeded to open the Steam app but error persisted.

It turns out that macOS aliases are not as flexible as unix symlinks[^3] in that they can't refer to files on a different partition. Good know but it seems like a somewhat counter-intuitive restriction for someone familiar with symlinks. So I decided to switch to a symlink instead.

```shell
$ ln -s /Volumes/<new-partition>/<steam-folder> ~/Library/Application Support
```

I opened the app again and this time and got a slightly different error message this time, progress!

> Steam requires that '/Applications/Steam.app/Contents/MacOS' be on a case-insensitive filesystem.

Sounds easy enough, many application don't actually depend on being installed in the `/Applications` folder, let's hope Steam is one of them. I moved Steam.app to the new partition as well and opened it from there instead and it worked! My friend was now able to log in and download the game.

The Steam support page also suggests installing Steam on another partition. As far I can see, there isn't an easy way to tell Steam to install to a specific place on macOS so I'm not exactly sure what they meant there.

## References

[^1]: <https://support.steampowered.com/kb_article.php?ref=8601-RYPX-5789>
[^2]: <https://support.apple.com/en-ca/guide/disk-utility/dskutl14027/mac>
[^3]: <https://en.wikipedia.org/wiki/Alias_(Mac_OS)#Relation_to_BSD_symbolic_and_hard_links>
