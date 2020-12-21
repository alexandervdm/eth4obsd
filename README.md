
This repository contains Ethereum related ports for OpenBSD. It currently only targets x86_64 architecture for OpenBSD 6.8 which was released on Oct 18, 2020. Most packages available here require obsd-specific patching at this time which is further detailed below.

All processes for a given client run under the same user account and as such it is assumed other methods of isolating the validator have been implemented.
It is recommended to follow the instructions in the provided README documents.

## Content

| Client | Version | Status |
| :---         |     :---      |          ---: |
| Geth    | 0.9.25            | ALL OK    |
| Prysm   | 1.0.5      | WORKING WITH PATCHES    |
| Lighthouse    | 1.0.4       | WORKING WITH PATCHES    |
| Nimbus        | N/A         | TBD |
| Teku          | N/A         | N/A    |

## How to use

Configure the ports tree for 6.8-stable using the steps below or this [guide](https://www.openbsd.org/faq/ports/ports.html):
```
cd /usr
cvs -qd anoncvs@anoncvs.eu.openbsd.org:/cvs checkout -rOPENBSD_6_8 -P ports
```

Configure this repository
```
cd /usr/ports
git clone https://github.com/alexandervdm/ethereum-openbsd.git
```

Modify the PORTSDIR_PATH in your /etc/mk.conf to include this repo, i.e:
```
PORTSDIR_PATH=${PORTSDIR}:${PORTSDIR}/ethereum-openbsd
```

Before building, add the appropriate (daemon) users to your ports user list:
```
cat >> /usr/ports/infrastructure/db/user.list << EOF
860 _geth               _geth           net/geth
861 _prysm              _prysm          net/prysm
862 _lighthouse         _lighthouse     net/lighthouse
EOF
```

Finally we can build our ports, for example geth:
```
cd /usr/ports/ethereum-openbsd/net/geth
make && make install
```

## Patches

Here follows an overview of the patching that were necessary to make the packages work on OpenBSD. I aim to make this list as small & boring as possible and welcome assistance towards this goal.

#### [Prysmaticlabs/Prysm](https://github.com/prysmaticlabs/prysm)
- Prysm uses Google's Bazel as a build tool. Bazel has an available OpenBSD [port](https://marc.info/?l=openbsd-ports&m=159163098121456&w=2), but while building prysm it tries to pull in components without OpenBSD support such as the llvm toolchain. At this time this prysm port avoids bazel all-together in favour of 'go build'.
- Prysm is built using a custom go-1.15.6 port to include recent go CVE fixes and match the Bazel build environment.
- Prysm depends on [herumi/bls](https://github.com/herumi/bls). Supported platforms pull in a pre-compiled static version of the library from [herumi/bls-eth-go-binary](https://github.com/herumi/bls-eth-go-binary) but OpenBSD is not one of them and I prefer compiling it myself anyway so a somewhat hacky port is provided under security/bls-eth-go.
- A set of patches was added to the Prysm build contraints for the new blst dependency.

#### [Sigp/Lighthouse](https://github.com/sigp/lighthouse)
- Build flag OPENSSL_NO_VENDOR was added to make use of the system libressl
- RUSTFLAGS="-C default_linker_libraries" was added to prevent linkage issues when compiling the lighthouse binary
- The slashing protection Makefile was patched to rename the GNU/tar calls to gtar

## Troubleshooting

If you are running into build failures, please ensure that the limits defined in /etc/login.conf for your user are adequate. Rust compiling in particular tends to require a lot of memory and will crash and burn if the default limits weren't adjusted.

## Disclaimer:

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED,
INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A
PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.



