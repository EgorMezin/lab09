# Лабораторная работа 09
## Тема:
Изучение процесса создания артефактов на примере GitHub Release
## Ход выполнения:
(строки со входными данными начинаются с $)
```bash
$ sudo apt install xclip
Installing:                     
  xclip

Summary:
  Upgrading: 0, Installing: 1, Removing: 0, Not Upgrading: 238
  Download size: 21.3 kB
  Space needed: 63.5 kB / 7,340 MB available

Get:1 http://deb.debian.org/debian trixie/main amd64 xclip amd64 0.13-4 [21.3 kB]
Fetched 21.3 kB in 0s (119 kB/s)  
Selecting previously unselected package xclip.
(Reading database ... 128341 files and directories currently installed.)
Preparing to unpack .../xclip_0.13-4_amd64.deb ...
Unpacking xclip (0.13-4) ...
Setting up xclip (0.13-4) ...
Processing triggers for man-db (2.13.1-1) ...

$ alias gsed=sed
$ alias pbcopy='xclip -selection clipboard'
$ alias pbpaste='xclip -selection clipboard -o'

$ git clone https://github.com/EgorMezin/lab08 projects/lab09
Cloning into 'projects/lab09'...
remote: Enumerating objects: 209, done.
remote: Counting objects: 100% (209/209), done.
remote: Compressing objects: 100% (96/96), done.
remote: Total 209 (delta 77), reused 209 (delta 77), pack-reused 0 (from 0)
Receiving objects: 100% (209/209), 46.18 KiB | 788.00 KiB/s, done.
Resolving deltas: 100% (77/77), done.

$ cd projects/lab09
$ git remote remove origin
$ git remote add origin https://EgorMezin/lab09
$ gsed -i 's/lab08/lab09/g' README.md

$ sudo apt install gpg
gpg is already the newest version (2.4.7-21+deb13u1+b3).
gpg set to manually installed.
Summary:
  Upgrading: 0, Installing: 0, Removing: 0, Not Upgrading: 238

$ gpg --full-generate-key
gpg (GnuPG) 2.4.7; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

gpg: keybox '/home/egor/.gnupg/pubring.kbx' created
Please select what kind of key you want:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (sign only)
  (14) Existing key from card
Your selection? 1
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 4096
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 0
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: EgorMezin
Email address: mjorg2007@gmail.com
Comment: comment
You selected this USER-ID:
    "EgorMezin (comment) <mjorg2007@gmail.com>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: /home/egor/.gnupg/trustdb.gpg: trustdb created
gpg: directory '/home/egor/.gnupg/openpgp-revocs.d' created
gpg: revocation certificate stored as '/home/egor/.gnupg/openpgp-revocs.d/8D2001A8DF6BA49474CB88C9507D84D8FDBBC988.rev'
public and secret key created and signed.

pub   rsa4096 2026-05-31 [SC]
      8D2001A8DF6BA49474CB88C9507D84D8FDBBC988
uid                      EgorMezin (comment) <mjorg2007@gmail.com>
sub   rsa4096 2026-05-31 [E]

$ gpg --list-secret-keys --keyid-format LONG
gpg: checking the trustdb
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
/home/egor/.gnupg/pubring.kbx
-----------------------------
sec   rsa4096/507D84D8FDBBC988 2026-05-31 [SC]
      8D2001A8DF6BA49474CB88C9507D84D8FDBBC988
uid                 [ultimate] EgorMezin (comment) <mjorg2007@gmail.com>
ssb   rsa4096/E85ADA99E3BD0939 2026-05-31 [E]

$ gpg -K EgorMezin
sec   rsa4096 2026-05-31 [SC]
      8D2001A8DF6BA49474CB88C9507D84D8FDBBC988
uid           [ultimate] EgorMezin (comment) <mjorg2007@gmail.com>
ssb   rsa4096 2026-05-31 [E]

$ GPG_KEY_ID=$(gpg --list-secret-keys --keyid-format LONG | grep sec | tail -1 | awk '{print $2}' | cut -d'/' -f2)
$ GPG_SEC_KEY_ID=$(gpg --list-secret-keys --keyid-format LONG | grep ssb | tail -1 | awk '{print $2}' | cut -d'/' -f2)

$ gpg --armor --export ${GPG_KEY_ID} | pbcopy
$ pbpaste
-----BEGIN PGP PUBLIC KEY BLOCK-----

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
WSq4pUw=
=YC8R
-----END PGP PUBLIC KEY BLOCK-----

$ open https://github.com/settings/keys
```
в настройках github добавляем gpg ключ
```bash
$ git config user.signingkey ${GPG_SEC_KEY_ID}
$ git config gpg.program gpg
$ test -r ~/.bash_profile && echo 'export GPG_TTY=$(tty)' >> ~/.bash_profile
$ echo 'export GPG_TTY=$(tty)' >> ~/.profile

$ cmake -H. -B_build -DCPACK_GENERATOR="TGZ"
CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.

CMake Warning (dev) at /usr/share/cmake-3.31/Modules/FetchContent.cmake:1373 (message):
  The DOWNLOAD_EXTRACT_TIMESTAMP option was not given and policy CMP0135 is
  not set.  The policy's OLD behavior will be used.  When using a URL
  download, the timestamps of extracted files should preferably be that of
  the time of extraction, otherwise code that depends on the extracted
  contents might not be rebuilt if the URL changes.  The OLD behavior
  preserves the timestamps from the archive instead, but this is usually not
  what you want.  Update your project to the NEW behavior or specify the
  DOWNLOAD_EXTRACT_TIMESTAMP option with a value of true to avoid this
  robustness issue.
Call Stack (most recent call first):
  CMakeLists.txt:5 (FetchContent_Declare)
This warning is for project developers.  Use -Wno-dev to suppress it.

-- The C compiler identification is GNU 14.2.0
-- The CXX compiler identification is GNU 14.2.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Found Python3: /usr/bin/python3 (found version "3.13.5") found components: Interpreter
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE
-- Configuring done (2.7s)
-- Generating done (0.0s)
-- Build files have been written to: /home/egor/EgorMezin/workspace/projects/lab09/_build

$ cmake --build _build --target package
[  6%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 12%] Linking CXX static library libprint.a
[ 12%] Built target print
[ 18%] Building CXX object _deps/googletest-build/googletest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
[ 25%] Linking CXX static library ../../../lib/libgtest.a
[ 25%] Built target gtest
[ 31%] Building CXX object _deps/googletest-build/googletest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 37%] Linking CXX static library ../../../lib/libgtest_main.a
[ 37%] Built target gtest_main
[ 43%] Building CXX object CMakeFiles/check.dir/tests/test_print.cpp.o
[ 50%] Linking CXX executable check
[ 50%] Built target check
[ 56%] Building CXX object CMakeFiles/demo.dir/demo/main.cpp.o
[ 62%] Linking CXX executable demo
[ 62%] Built target demo
[ 68%] Building CXX object _deps/googletest-build/googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 75%] Linking CXX static library ../../../lib/libgmock.a
[ 75%] Built target gmock
[ 81%] Building CXX object _deps/googletest-build/googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[ 87%] Linking CXX static library ../../../lib/libgmock_main.a
[ 87%] Built target gmock_main
[ 93%] Building CXX object tests/CMakeFiles/tests.dir/test_print.cpp.o
[100%] Linking CXX executable tests
[100%] Built target tests
Run CPack packaging tool...
CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print []
CPack: Create package
CPack: - package: /home/egor/EgorMezin/workspace/projects/lab09/_build/print--Linux.tar.gz generated.

$ git tag -s v0.1.0.0 -m"Initial release v0.1.0.0"

$ git tag -v v0.1.0.0
object 4b0feaeb8a77eea7dd805fd7a6bf205f6788991b
type commit
tag v0.1.0.0
tagger EgorMezin <mjorg2007@gmail.com> 1780256036 -0400

Initial release v0.1.0.0
gpg: Signature made Sun 31 May 2026 03:33:56 PM EDT
gpg:                using RSA key 8D2001A8DF6BA49474CB88C9507D84D8FDBBC988
gpg: Good signature from "EgorMezin (comment) <mjorg2007@gmail.com>" [ultimate]

$ git show v0.1.0.0
tag v0.1.0.0
Tagger: EgorMezin <mjorg2007@gmail.com>
Date:   Sun May 31 15:33:56 2026 -0400

Initial release v0.1.0.0
-----BEGIN PGP SIGNATURE-----

iQIzBAABCgAdFiEEjSABqN9rpJR0y4jJUH2E2P27yYgFAmocjSQACgkQUH2E2P27
yYjjvhAAtguPteZ5TLnX3msyfaSq5dxVu1l2FzCBe9QFGBI6a4trcwePWp7tJ23/
NbIw9qgwimE/D/GqVHU3nCSjmiEOoL3IWN+xb2/LKi1fqrrpuRbZ4+IEwMitE8qt
yFVB9kBohjGq5vZNAVf+M9scaxaZoZ2bAyYbowhBNUufv6rjgJhCnKJ9YqY8vwkO
Lb0SbikTe0V8t7+uWBI3dg0u4cmNZTD8JA7FplpEKT2aV1lxDJh6c19pr2upHMHp
wDFknV3A6xKodvZ05PfKoOuVYf5AuwR4r+p9Q6M+MPdky2vCiUUPwb410XgmdxIG
GjfdP1fd/Y/QUfJbgie5RPJxDVkcM3gR9RPhHgce9z/NH2NEEGSOZp1XcHrYEy5b
DcoHpXtIaBNk6/lefNhFriJWhzQg0lK6XJZgmnJiiIAbu+AXIBZaJ0/VHi00B/9U
C1eokiX4uktlAGZywFy7ro3PTwapD6WKTAzHhAFQ7rSf2Xbdqhuq5QhsxEQiS5En
c8yNVQgUad93qqD1hgBjERBCecGo2XkKtKg2FObdkDKhaeiDQoc99rn7j2pbUHpW
zFam43+tNLYiH5lFxiH56ocIO7KcHi7xfIYY7kEgcJ0Hi3wgeHvD11UsU8Ye+XOA
VvSv0/2ZZ1LfoIrYqUmxudfxdhuBi/Vc2NC41KtLRqjxfeqr73o=
=KQtd
-----END PGP SIGNATURE-----

$ git push origin main --tags
Enumerating objects: 210, done.
Counting objects: 100% (210/210), done.
Delta compression using up to 2 threads
Compressing objects: 100% (97/97), done.
Writing objects: 100% (210/210), 46.94 KiB | 23.47 MiB/s, done.
Total 210 (delta 77), reused 209 (delta 77), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (77/77), done.
To https://github.com/EgorMezin/lab09
 * [new branch]      main -> main
 * [new tag]         v0.1.0.0 -> v0.1.0.0

$ gh release create v0.1.0.0 --title "libprint" --notes "my first release" _build/*.tar.gz
https://github.com/EgorMezin/lab09/releases/tag/v0.1.0.0

$ gh release list
TITLE     TYPE    TAG NAME  PUBLISHED          
libprint  Latest  v0.1.0.0  about 8 minutes ago

$ gh release view v0.1.0.0
v0.1.0.0
EgorMezin released this less than a minute ago

  my first release                                                                


Assets
print--Linux.tar.gz  724.99 KiB

View on GitHub: https://github.com/EgorMezin/lab09/releases/tag/v0.1.0.0

$ gh release download v0.1.0.0 --pattern "*.tar.gz"
$ tar -zxf print--Linux.tar.gz

$ tar -tzf print--Linux.tar.gz
print--Linux/include/
print--Linux/include/gtest/
print--Linux/include/gtest/gtest-death-test.h
print--Linux/include/gtest/gtest-matchers.h
print--Linux/include/gtest/gtest_prod.h
print--Linux/include/gtest/gtest-param-test.h
print--Linux/include/gtest/internal/
print--Linux/include/gtest/internal/gtest-port-arch.h
print--Linux/include/gtest/internal/gtest-port.h
print--Linux/include/gtest/internal/gtest-internal.h
print--Linux/include/gtest/internal/custom/
print--Linux/include/gtest/internal/custom/README.md
print--Linux/include/gtest/internal/custom/gtest-port.h
print--Linux/include/gtest/internal/custom/gtest-printers.h
print--Linux/include/gtest/internal/custom/gtest.h
print--Linux/include/gtest/internal/gtest-type-util.h
print--Linux/include/gtest/internal/gtest-param-util.h
print--Linux/include/gtest/internal/gtest-filepath.h
print--Linux/include/gtest/internal/gtest-death-test-internal.h
print--Linux/include/gtest/internal/gtest-string.h
print--Linux/include/gtest/gtest-printers.h
print--Linux/include/gtest/gtest-spi.h
print--Linux/include/gtest/gtest-message.h
print--Linux/include/gtest/gtest-typed-test.h
print--Linux/include/gtest/gtest-assertion-result.h
print--Linux/include/gtest/gtest_pred_impl.h
print--Linux/include/gtest/gtest-test-part.h
print--Linux/include/gtest/gtest.h
print--Linux/include/print.hpp
print--Linux/include/gmock/
print--Linux/include/gmock/gmock.h
print--Linux/include/gmock/internal/
print--Linux/include/gmock/internal/gmock-pp.h
print--Linux/include/gmock/internal/gmock-internal-utils.h
print--Linux/include/gmock/internal/custom/
print--Linux/include/gmock/internal/custom/README.md
print--Linux/include/gmock/internal/custom/gmock-generated-actions.h
print--Linux/include/gmock/internal/custom/gmock-matchers.h
print--Linux/include/gmock/internal/custom/gmock-port.h
print--Linux/include/gmock/internal/gmock-port.h
print--Linux/include/gmock/gmock-actions.h
print--Linux/include/gmock/gmock-function-mocker.h
print--Linux/include/gmock/gmock-matchers.h
print--Linux/include/gmock/gmock-more-matchers.h
print--Linux/include/gmock/gmock-spec-builders.h
print--Linux/include/gmock/gmock-cardinalities.h
print--Linux/include/gmock/gmock-more-actions.h
print--Linux/include/gmock/gmock-nice-strict.h
print--Linux/cmake/
print--Linux/cmake/print-config-noconfig.cmake
print--Linux/cmake/print-config.cmake
print--Linux/bin/
print--Linux/bin/demo
print--Linux/lib/
print--Linux/lib/libprint.a
print--Linux/lib/libgmock.a
print--Linux/lib/libgmock_main.a
print--Linux/lib/cmake/
print--Linux/lib/cmake/GTest/
print--Linux/lib/cmake/GTest/GTestTargets.cmake
print--Linux/lib/cmake/GTest/GTestConfigVersion.cmake
print--Linux/lib/cmake/GTest/GTestConfig.cmake
print--Linux/lib/cmake/GTest/GTestTargets-noconfig.cmake
print--Linux/lib/pkgconfig/
print--Linux/lib/pkgconfig/gmock.pc
print--Linux/lib/pkgconfig/gtest.pc
print--Linux/lib/pkgconfig/gmock_main.pc
print--Linux/lib/pkgconfig/gtest_main.pc
print--Linux/lib/libgtest_main.a
print--Linux/lib/libgtest.a
```