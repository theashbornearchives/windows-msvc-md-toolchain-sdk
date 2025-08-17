# windows-msvc-md-toolchain-sdk
Docs-only. Windows C/C++ linking SDK for MSVC v143 (x64): /MD aligned, static-first with a tiny declared DLL set (idn2/unistring/psl/uv; zlib optional). Includes CMake &amp; Ninja; prebuilt libs: Abseil, Protobuf/gRPC, curl+OpenSSL, zlib/zstd, Brotli, nghttp2, libssh2, libuv, libarchive, c-ares, RE2. Relocatable, dumpbin-verified. Paid access.

Windows MSVC /MD Toolchain SDK · Docs Only

Type: Windows C/C++ linking SDK for MSVC v143 (VS 2022), x64
 Runtime: /MD (MultiThreadedDLL) alignment across the stack
 Packaging: Static-first libraries + clean headers with a small, declared DLL set: libidn2-0.dll, libunistring-5.dll, libpsl-5.dll, uv.dll (this edition also ships zlib.dll; zlibstatic.lib provided)
 Status: This repo is documentation only. Binaries, install trees, and scripts are offered privately.
The 3 Guarantees
MSVC-only & /MD-aligned — no /MT bleed; LNK2038 minimized


Static-first with an explicit DLL set — DLLs for legal/technical reasons only


Reproducible & verified — dumpbin/Dependencies checks and a consistent, relocatable layout


Why this exists
Windows C++ teams lose days to /MT vs /MD mismatches, hidden in-tree builds, and DLL creep. This SDK is a drop-in linking stack: unzip → point CMake at bin/, lib/, include/ → link. No vcpkg/Conan required.
 Not a compiler or cross-compiler. You bring MSVC v143 and a Windows 10+ SDK; this provides the libraries, headers, and tools that link cleanly together under /MD.
Why it’s different
/MD runtime alignment verified with dumpbin + link diagnostics


Static libraries + clean headers for drop-in CI and reproducible builds


Dependencies externalized — no hidden in-tree builds


Relocatable layout: bin/, lib/, include/, certs/


Built with CMake + Ninja and validated end-to-end
What’s in the private bundle
Tools (bin/)
CMake: cmake.exe, ctest.exe, cpack.exe


Ninja: ninja.exe


Protocol Buffers & gRPC:
 protoc.exe, grpc_cpp_plugin.exe, grpc_csharp_plugin.exe, grpc_node_plugin.exe,
 grpc_objective_c_plugin.exe, grpc_php_plugin.exe, grpc_python_plugin.exe, grpc_ruby_plugin.exe


OpenSSL: openssl.exe


cURL: curl.exe


XZ/LZMA: xz.exe, xzdec.exe


Assembler / Generator: nasm.exe, re2c.exe


CPython (x64): python.exe (+ stdlib; built from PCbuild\amd64)


All tools are built for MSVC v143 (VS 2022), x64, and aligned to /MD.
Libraries (lib/ + include/)
Core / Compression / Parsing: abseil, brotli, bzip2, expat, xz/lzma, zlib, libzstd


Networking / HTTP / Crypto: c-ares, nghttp2, libssh2, OpenSSL 3, curl


Event / FS / Archive: libuv, libarchive


Google stack: protobuf, gRPC, RE2


Charsets / IDN / PSL: libiconv, libidn2, libpsl, libunistring


Testing: gtest/gmock (without Abseil) and gtest/gmock (with Abseil integrated)


Policy: /MD (MultiThreadedDLL) across the stack; static-first delivery (static .lib + headers), with import .lib provided wherever a DLL is supplied.
Declared DLL set (present in bin/)
Small, explicit dynamic surface shipped by design:
libidn2-0.dll


libunistring-5.dll


libpsl-5.dll


uv.dll


zlib.dll (static zlibstatic.lib also included)


All other listed libraries are provided as static .lib (with headers). Import libraries for the above DLLs are present in lib/.

Requirements
Windows 10/11 (x64)


MSVC v143 (Visual Studio 2022) and Windows 10 SDK


VC++ Redistributable recommended for end-users (installer: VC_redist.x64.exe).
 Portable ZIPs with local CRT DLLs are possible, but the redist is preferred for security updates.


HTTPS & certificates (curl/OpenSSL)
Ship certs\cacert.pem (Mozilla CA bundle).


Overrides:


Env: CURL_CA_BUNDLE, SSL_CERT_FILE


Build-time (curl): -DCURL_CA_BUNDLE=..., -DCURL_CA_PATH=...


Per-app (libcurl): CURLOPT_CAINFO, CURLOPT_CAPATH


You can use CAPATH (hashed PEM directory) or embed certs into your EXE for a single-binary workflow.


What it is / what it isn’t
Is
A prebuilt, relocatable linking SDK for MSVC v143 /MD, with tools: CMake, Ninja, protoc, gRPC plugins, OpenSSL/curl, nasm, re2c, xz.


Designed for CI, game/tool pipelines, SDK maintainers, and offline/security-sensitive environments.


Static-first with a tiny declared DLL set and verifiable boundaries.


Isn’t
Not a compiler or MinGW/Clang cross-toolchain.


Not vcpkg/Conan (no online resolution; everything is externalized).


Not a free binary mirror — this repo is docs-only; access is paid.

Licensing & attribution: 
See licenses/THIRD_PARTY.md for the component-to-license map (full license texts ship inside the paid SDK). For LGPL/GPL–touched components such as libidn2 and libunistring, I do not statically link them in proprietary targets; they are shipped as DLLs with import libs or omitted entirely depending on edition/SKU.


Access & support: 
Access to binaries, install trees, and build scripts is paid. 

Contact me at stillstanding.stillburning@gmail.com. 

A Gumroad store page is coming soon; the purchase link will be added here when live. Public questions should go to GitHub Discussions under the “Access Questions” category. Formal requests (access, trials, quotes) should be filed as a GitHub Issue using the “Access request” template. For private or sensitive topics (pricing, invoices, security reviews), email is preferred.
FAQ
Q: Is this vcpkg/Conan?
 A: No. It’s a prebuilt, reproducible /MD linking SDK you unzip and use. In SKUs that include export files, set CMAKE_PREFIX_PATH and use find_package(... CONFIG).
Q: Does it include a compiler?
 A: No. You bring MSVC v143 (VS 2022) and the Windows SDK.
Q: /MT support?
 A: A separate /MT edition is available on special request, but has to be built, and it would take weeks.
Q: End-user runtime?
 A: Prefer the official VC++ Redistributable for security updates. Portable ZIPs with local CRT DLLs are also supported for no-installer workflows.
Q: Which DLLs am I expected to ship?
 A: Only the declared set (e.g., libidn2-0.dll, libunistring-5.dll, libpsl-5.dll, uv.dll; optional zlib.dll if you choose DLL for zlib). Everything else links static by default. Import libs for these DLLs are in lib/.
