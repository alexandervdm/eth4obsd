
This repo contains Ethereum related ports for OpenBSD. It currently targets x86_64 only for OpenBSD 6.8-beta which should be released at some point in Q3 2020. Most packages available here require obsd-specific patching at this time which is further detailed below.


## Content

| Client | Version | Status |
| :---         |     :---      |          ---: |
| Geth    | 0.9.23            | ALL OK    |
| Prysm   | 1.0.0-alpha29     | WORKING WITH PATCHES    |
| Lighthouse    | 0.3.1       | WORKING WITH PATCHES    |
| Nimbus        | N/A         | TBD |
| Teku          | N/A         | N/A    |

## How to use

Configure the ports tree for -current using the steps below or this [guide](https://www.openbsd.org/faq/ports/ports.html):
```
cd /usr
cvs -qd anoncvs@anoncvs.eu.openbsd.org:/cvs checkout -P ports
```

Configure this repository
```
cd /usr/ports
git clone https://github.com/alexandervdm/eth4obsd.git
```

Modify the PORTSDIR_PATH in your /etc/mk.conf to include this repo, i.e:
```
PORTSDIR_PATH=${PORTSDIR}:${PORTSDIR}/eth4obsd
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
cd /usr/ports/eth4obsd/net/geth
make && make install
```

## Patches

Here follows an overview of the patching that were necessary to make the packages work on OpenBSD. I aim to make this list as small & boring as possible and welcome assistance towards this goal.

#### [Prysmaticlabs/Prysm](https://github.com/prysmaticlabs/prysm)
- Prysm uses Google's Bazel as a build tool. Bazel has an available OpenBSD [port](https://marc.info/?l=openbsd-ports&m=159163098121456&w=2), but while building prysm it tries to pull in components without OpenBSD support such as the llvm toolchain. At this time this prysm port avoids bazel in favour of 'go build'.
- Prysm is built using a custom go-1.14 port to match the Bazel build environment. Also binaries built using the system go-1.15 seem to have peering issues. Need to investigate further.
- Prysm depends on [herumi/bls](https://github.com/herumi/bls). Supported platforms pull in a pre-compiled static version of the library from [herumi/bls-eth-go-binary](https://github.com/herumi/bls-eth-go-binary) but OpenBSD is not one of them and I prefer compiling it myself anyway so a somewhat hacky port is provided under security/bls-eth-go.
- A set of patches was added to the Prysm build contraints for the new blst dependency.

#### [Sigp/Lighthouse](https://github.com/sigp/lighthouse)
- Dependency openssl-sys-0.5.8 was forked to include a [patch](https://github.com/alexandervdm/rust-openssl) that adds support for LibreSSL 3.2.1
- Dependency tokio-uds-0.1.7 was forked to include a cherry-picked [patch](https://github.com/alexandervdm/tokio-uds/commits/017-for-openbsd) that adds OpenBSD support
- Build flag OPENSSL_NO_VENDOR was added to make use of the system's libressl
- RUSTFLAGS="-C default_linker_libraries" was added to prevent fatal linkage issues when compiling the lighthouse binary

## Troubleshooting

If you are running into build failures, please ensure that the limits defined in /etc/login.conf for your user are adequate. Rust compiling in particular tends to require a lot of memory and will crash and burn if haven't adjusted the default limits.

## Disclaimer:

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED,
INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A
PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.



