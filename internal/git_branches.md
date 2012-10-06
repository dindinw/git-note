Git Branches
============

Git show-branch confusion
-------------------------
We use the git itself for example (checkout from git://github.com/git/git.git)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
$ git rev-parse master
f84667def209e4a84e37e8488a08e9eca3f208c1

$ git show-branch --more=15 f84667de
[f84667de] Update draft release notes to 1.8.0
[f84667de^] Merge branch 'nd/grep-reflog'
[f84667de^^2] revision: make --grep search in notes too if shown
[f84667de~2] Merge branch 'lt/mailinfo-handle-attachment-more-sanely'
[f84667de~2^2] mailinfo: don't require "text" mime type for attachments
[f84667de~3] Merge branch 'tu/gc-auto-quiet'
[f84667de~3^2] silence git gc --auto --quiet output
[f84667de~4] Merge branch 'maint'
[f84667de~4^2] Start preparing for 1.7.12.3
[f84667de~4^2^] Merge branch 'rr/maint-submodule-unknown-cmd' into maint
[f84667de~4^2^^2] submodule: if $command was not matched, don't parse other args
[f84667de~4^2~2] Merge branch 'sp/maint-http-enable-gzip' into maint
[f84667de~4^2~2^2] Enable info/refs gzip decompression in HTTP client
[f84667de~4^2~3] Merge branch 'sp/maint-http-info-refs-no-retry' into maint
[f84667de~4^2~3^2] Revert "retry request without query when info/refs?query fails"
[f84667de~4^2~4] l10n: Fix to Swedish translation

$ git show-branch --more=15
[master] Update draft release notes to 1.8.0
[master^] Merge branch 'nd/grep-reflog'
[master^^2] revision: make --grep search in notes too if shown
[master~2] Merge branch 'lt/mailinfo-handle-attachment-more-sanely'
[master~2^2] mailinfo: don't require "text" mime type for attachments
[master~3] Merge branch 'tu/gc-auto-quiet'
[master~3^2] silence git gc --auto --quiet output
[master~4] Merge branch 'maint'
[master~4^2] Start preparing for 1.7.12.3
[master~4^2^] Merge branch 'rr/maint-submodule-unknown-cmd' into maint
[master~4^2^^2] submodule: if $command was not matched, don't parse other args
[master~4^2~2] Merge branch 'sp/maint-http-enable-gzip' into maint
[master~4^2~2^2] Enable info/refs gzip decompression in HTTP client
[master~4^2~3] Merge branch 'sp/maint-http-info-refs-no-retry' into maint
[master~4^2~3^2] Revert "retry request without query when info/refs?query fails"
[master~4^2~4] l10n: Fix to Swedish translation

$ git log -15 --pretty=oneline master
f84667def209e4a84e37e8488a08e9eca3f208c1 Update draft release notes to 1.8.0
fa11d7c87940154f3f6463353335c6010b13e5db Merge branch 'nd/grep-reflog'
5ce993a8126e4f428bc7862d4120b3ae81eb6a79 Merge branch 'lt/mailinfo-handle-attachment-more-sanely'
9ac54d0f59978b92201e3ca728eed0ea7545667a Merge branch 'tu/gc-auto-quiet'
b65f30b6b3bf34831b32a1b209bc1955f2bf79df Merge branch 'maint'
9376c8603fc1f9b5bf663b76705dfee77f71ef82 Start preparing for 1.7.12.3
e2c7a5b646bb9a4f126577346697d79b3143d30b Merge branch 'rr/maint-submodule-unknown-cmd' into maint
0a65df58a0d5b17a72edd4e6247be29ab8af2b09 Merge branch 'sp/maint-http-enable-gzip' into maint
8a477ddf237978d3f5725b24e286616000ca2ffd Merge branch 'sp/maint-http-info-refs-no-retry' into maint
a9073097b984a2a1db4f3b0df86cfaf624dfef21 l10n: Fix to Swedish translation
1ec6f488de495a3b77b6a6b6fb42919f64a4bff0 Documentation: mention `push.default` in git-push.txt
d117dd20961c132b9db780c72a78ca8ecf7508eb RelNotes/1.8.0: various typo and style fixes
b0ec16b49eb283156e13bbef26466d948e4fd992 Git 1.8.0-rc0
abc05cbcd3fdc6e5e14daec80c00b6f51b8e4c7e Merge branch 'jk/completion-tests'
70dac5f44d60a3f2de68c17157c21e663fc75616 Merge branch 'ep/malloc-check-perturb'
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 

The master is `f84667de`

We show the branches since `f84467de` for 15 lines by using `--more` option.

Branches  diffrent than `git log` for 15 lines.
 * `f84667de` is `master`
 * `falld7c8` is `master^`
 * `5ce993a8` is `master~2`
 * `9ac54d0f` is `master~3`
 * `b65f30b6` is `master~4`
 * `9376c860` is `master~4^2`
 * `e2c7a5b6` is `master~4^2^``
We can find the log show not only the main-stream of code as my first idea. Actually it shows the time-line.
(The commits list by commit time)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
$ git log -15 --topo-order --pretty=oneline master
f84667def209e4a84e37e8488a08e9eca3f208c1 Update draft release notes to 1.8.0
fa11d7c87940154f3f6463353335c6010b13e5db Merge branch 'nd/grep-reflog'
38cfe915bfc3ea0dbcbedaa82c44460a2ada2f42 revision: make --grep search in notes too if shown
baa6378ff2106738c74213280904507d0ed8129c log --grep-reflog: reject the option without -g
72fd13f71c18b438ca3e482c126bcbcaa2dac650 revision: add --grep-reflog to filter commits by reflog messages
ad4813b3c2513c5dc7e84305ab8a393b32124977 grep: prepare for new header field filter
5ce993a8126e4f428bc7862d4120b3ae81eb6a79 Merge branch 'lt/mailinfo-handle-attachment-more-sanely'
9d55b2e12f82b344709b0f2981eaf142b0b1f7a8 mailinfo: don't require "text" mime type for attachments
9ac54d0f59978b92201e3ca728eed0ea7545667a Merge branch 'tu/gc-auto-quiet'
df995c7dd21bca9c61f9e5480fdfc1a015b4f1a0 silence git gc --auto --quiet output
b65f30b6b3bf34831b32a1b209bc1955f2bf79df Merge branch 'maint'
9376c8603fc1f9b5bf663b76705dfee77f71ef82 Start preparing for 1.7.12.3
e2c7a5b646bb9a4f126577346697d79b3143d30b Merge branch 'rr/maint-submodule-unknown-cmd' into maint
0a65df58a0d5b17a72edd4e6247be29ab8af2b09 Merge branch 'sp/maint-http-enable-gzip' into maint
8a477ddf237978d3f5725b24e286616000ca2ffd Merge branch 'sp/maint-http-info-refs-no-retry' into maint
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 

This time, we add `--topo-order` option, we found:
 * `f84667de` is `master`
 * `falld7c8` is `master^`
 * `38cfe991` is `master^^2` 

It's still different than the `git show-branch`

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
$ git log -15 --pretty=oneline --topo-order master 
f84667def209e4a84e37e8488a08e9eca3f208c1 Update draft release notes to 1.8.0
fa11d7c87940154f3f6463353335c6010b13e5db Merge branch 'nd/grep-reflog'
38cfe915bfc3ea0dbcbedaa82c44460a2ada2f42 revision: make --grep search in notes too if shown
baa6378ff2106738c74213280904507d0ed8129c log --grep-reflog: reject the option without -g
72fd13f71c18b438ca3e482c126bcbcaa2dac650 revision: add --grep-reflog to filter commits by reflog messages
ad4813b3c2513c5dc7e84305ab8a393b32124977 grep: prepare for new header field filter
5ce993a8126e4f428bc7862d4120b3ae81eb6a79 Merge branch 'lt/mailinfo-handle-attachment-more-sanely'
9d55b2e12f82b344709b0f2981eaf142b0b1f7a8 mailinfo: don't require "text" mime type for attachments
9ac54d0f59978b92201e3ca728eed0ea7545667a Merge branch 'tu/gc-auto-quiet'
df995c7dd21bca9c61f9e5480fdfc1a015b4f1a0 silence git gc --auto --quiet output
b65f30b6b3bf34831b32a1b209bc1955f2bf79df Merge branch 'maint'
9376c8603fc1f9b5bf663b76705dfee77f71ef82 Start preparing for 1.7.12.3
e2c7a5b646bb9a4f126577346697d79b3143d30b Merge branch 'rr/maint-submodule-unknown-cmd' into maint
0a65df58a0d5b17a72edd4e6247be29ab8af2b09 Merge branch 'sp/maint-http-enable-gzip' into maint
8a477ddf237978d3f5725b24e286616000ca2ffd Merge branch 'sp/maint-http-info-refs-no-retry' into maint

$ git log -15 --pretty=oneline --graph master
* f84667def209e4a84e37e8488a08e9eca3f208c1 Update draft release notes to 1.8.0
*   fa11d7c87940154f3f6463353335c6010b13e5db Merge branch 'nd/grep-reflog'
|\  
| * 38cfe915bfc3ea0dbcbedaa82c44460a2ada2f42 revision: make --grep search in notes too if shown
| * baa6378ff2106738c74213280904507d0ed8129c log --grep-reflog: reject the option without -g
| * 72fd13f71c18b438ca3e482c126bcbcaa2dac650 revision: add --grep-reflog to filter commits by reflog messages
| * ad4813b3c2513c5dc7e84305ab8a393b32124977 grep: prepare for new header field filter
* |   5ce993a8126e4f428bc7862d4120b3ae81eb6a79 Merge branch 'lt/mailinfo-handle-attachment-more-sanely'
|\ \  
| * | 9d55b2e12f82b344709b0f2981eaf142b0b1f7a8 mailinfo: don't require "text" mime type for attachments
* | |   9ac54d0f59978b92201e3ca728eed0ea7545667a Merge branch 'tu/gc-auto-quiet'
|\ \ \  
| * | | df995c7dd21bca9c61f9e5480fdfc1a015b4f1a0 silence git gc --auto --quiet output
* | | |   b65f30b6b3bf34831b32a1b209bc1955f2bf79df Merge branch 'maint'
|\ \ \ \  
| * | | | 9376c8603fc1f9b5bf663b76705dfee77f71ef82 Start preparing for 1.7.12.3
| * | | |   e2c7a5b646bb9a4f126577346697d79b3143d30b Merge branch 'rr/maint-submodule-unknown-cmd' into maint
| |\ \ \ \  
| * \ \ \ \   0a65df58a0d5b17a72edd4e6247be29ab8af2b09 Merge branch 'sp/maint-http-enable-gzip' into maint
| |\ \ \ \ \  
| * \ \ \ \ \   8a477ddf237978d3f5725b24e286616000ca2ffd Merge branch 'sp/maint-http-info-refs-no-retry' into maint
| |\ \ \ \ \ \  
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 

This time, we see the output of git log is same when using `--topo-order` comparing with `--graph`
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
$ git show-branch --topo-order --more=117 |head -15
[master] Update draft release notes to 1.8.0
[master^] Merge branch 'nd/grep-reflog'
[master^^2] revision: make --grep search in notes too if shown
[master^^2^] log --grep-reflog: reject the option without -g
[master^^2~2] revision: add --grep-reflog to filter commits by reflog messages
[master^^2~3] grep: prepare for new header field filter
[master~2] Merge branch 'lt/mailinfo-handle-attachment-more-sanely'
[master~2^2] mailinfo: don't require "text" mime type for attachments
[master~3] Merge branch 'tu/gc-auto-quiet'
[master~3^2] silence git gc --auto --quiet output
[master~4] Merge branch 'maint'
[master~4^2] Start preparing for 1.7.12.3
[master~4^2^] Merge branch 'rr/maint-submodule-unknown-cmd' into maint
[master~4^2~2] Merge branch 'sp/maint-http-enable-gzip' into maint
[master~4^2~3] Merge branch 'sp/maint-http-info-refs-no-retry' into maint
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 

We find when use `--more=117` (or bigger), the head 15 line of the output of `git show-branch` is same with `git log`.
So we got a idea that the `--more` option can effect the output. 
 
So, why?
--------

`git show-branch` will traverse through all the commits on all branches being shown and stop when it find the most recent common commit which present on all of branches.
That is the reason that if you privide only one branch name, it will show only the branch head commit.

`--more=n` option tell how many additional commits you want to see after the most recent common commit in time along. 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
$ git show-branch --more=38 master                                                        $ git show-branch --more=37 master
[master] Update draft release notes to 1.8.0                                              [master] Update draft release notes to 1.8.0
[master^] Merge branch 'nd/grep-reflog'                                                   [master^] Merge branch 'nd/grep-reflog'
[master^^2] revision: make --grep search in notes too if shown                            [master^^2] revision: make --grep search in notes too if shown
[master~2] Merge branch 'lt/mailinfo-handle-attachment-more-sanely'                       [master~2] Merge branch 'lt/mailinfo-handle-attachment-more-sanely'
[master~2^2] mailinfo: don't require "text" mime type for attachments                     [master~2^2] mailinfo: don't require "text" mime type for attachments
[master~2^2^] Git 1.7.9.7                                                                 [master~2^2^] Git 1.7.9.7
[master~3] Merge branch 'tu/gc-auto-quiet'                                                [master~3] Merge branch 'tu/gc-auto-quiet'
[master~3^2] silence git gc --auto --quiet output                                         [master~3^2] silence git gc --auto --quiet output
[master~4] Merge branch 'maint'                                                           [master~4] Merge branch 'maint'
[master~4^2] Start preparing for 1.7.12.3                                                 [master~4^2] Start preparing for 1.7.12.3
[master~4^2^] Merge branch 'rr/maint-submodule-unknown-cmd' into maint                    [master~4^2^] Merge branch 'rr/maint-submodule-unknown-cmd' into maint
[master~4^2~2] Merge branch 'sp/maint-http-enable-gzip' into maint                        [master~4^2^^2] submodule: if $command was not matched, don't parse other args
[master~4^2~2^2] Enable info/refs gzip decompression in HTTP client                       [master~4^2~2] Merge branch 'sp/maint-http-enable-gzip' into maint
[master~4^2~3] Merge branch 'sp/maint-http-info-refs-no-retry' into maint                 [master~4^2~2^2] Enable info/refs gzip decompression in HTTP client
[master~4^2~3^2] Revert "retry request without query when info/refs?query fails"          [master~4^2~3] Merge branch 'sp/maint-http-info-refs-no-retry' into maint
[master~4^2~4] l10n: Fix to Swedish translation                                           [master~4^2~3^2] Revert "retry request without query when info/refs?query fails"
[master~5] Documentation: mention `push.default` in git-push.txt                          [master~4^2~4] l10n: Fix to Swedish translation
[master~6] RelNotes/1.8.0: various typo and style fixes                                   [master~5] Documentation: mention `push.default` in git-push.txt
[master~7] Git 1.8.0-rc0                                                                  [master~6] RelNotes/1.8.0: various typo and style fixes
[master~8] Merge branch 'jk/completion-tests'                                             [master~7] Git 1.8.0-rc0
[master~8^2] t9902: add completion tests for "odd" filenames                              [master~8] Merge branch 'jk/completion-tests'
[master~9] Merge branch 'ep/malloc-check-perturb'                                         [master~8^2] t9902: add completion tests for "odd" filenames
[master~9^2] MALLOC_CHECK: enable it, unless disabled explicitly                          [master~9] Merge branch 'ep/malloc-check-perturb'
[master~10] Merge branch 'da/mergetool-custom'                                            [master~9^2] MALLOC_CHECK: enable it, unless disabled explicitly
[master~10^2] mergetool--lib: Allow custom commands to override built-ins                 [master~10] Merge branch 'da/mergetool-custom'
[master~11] Merge branch 'os/commit-submodule-ignore'                                     [master~10^2] mergetool--lib: Allow custom commands to override built-ins
[master~11^2] commit: pay attention to submodule.$name.ignore in .gitmodules              [master~11] Merge branch 'os/commit-submodule-ignore'
[master~12] Merge branch 'jc/blame-follows-renames'                                       [master~11^2] commit: pay attention to submodule.$name.ignore in .gitmodules
[master~12^2] git blame: document that it always follows origin across whole-file renames [master~12] Merge branch 'jc/blame-follows-renames'
[master~13] Merge branch 'jk/receive-pack-unpack-error-to-pusher'                         [master~12^2] git blame: document that it always follows origin across whole-file renames
[master~13^2] receive-pack: drop "n/a" on unpacker errors                                 [master~13] Merge branch 'jk/receive-pack-unpack-error-to-pusher'
[master~14] Merge branch 'rt/maint-clone-single'                                          [master~13^2] receive-pack: drop "n/a" on unpacker errors
[master~14^2] clone --single: limit the fetch refspec to fetched branch                   [master~14] Merge branch 'rt/maint-clone-single'
[master~15] Merge git://github.com/git-l10n/git-po                                        [master~14^2] clone --single: limit the fetch refspec to fetched branch
[master~15^2] Merge git://github.com/gotgit/git-po-zh_CN                                  [master~15] Merge git://github.com/git-l10n/git-po
[master~16] Update draft release notes to 1.8.0                                           [master~15^2] Merge git://github.com/gotgit/git-po-zh_CN
[master~17] Sync with 1.7.12.2                                                            [master~16] Update draft release notes to 1.8.0
[master~4^2~5] Git 1.7.12.2                                                               [master~17] Sync with 1.7.12.2
[master~18] Merge branch 'rr/maint-submodule-unknown-cmd'                                 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 

From 37 to 38, [master~4^2^^2] removed, and [master~4^2~5],[master~18] added. Why, it's confused, I suppose only [master~18] is added.
So what's reason of the substitution from [master~4^2^^2] to [master~4^2~5] ?
 

