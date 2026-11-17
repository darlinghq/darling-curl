project(libcurl)

cmake_minimum_required(VERSION 3.13)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
	cmake_policy(SET CMP0005 NEW)
endif(COMMAND cmake_policy)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -nostdinc -D__DARWIN_UNIX03 -fPIC -w")

# config_darling/curl_config.h is Apple's config_mac/curl_config.h with
# SecureTransport disabled: LibreSSL is the only TLS backend.
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/config_darling)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/curl/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/curl/lib)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/curl/src)
include_directories(${CMAKE_CURRENT_SOURCE_DIR})

add_definitions(-DHAVE_CONFIG_H=1)
add_definitions(-DOS="x86_64-apple-darwin")
add_definitions(-DCLIENTNAME="libcurl")

set(curl_SRCS
	curl/lib/vauth/cleartext.c
	curl/lib/vauth/cram.c
	curl/lib/vauth/digest.c
	curl/lib/vauth/krb5_gssapi.c
	curl/lib/vauth/ntlm.c
	curl/lib/vauth/oauth2.c
	curl/lib/vauth/spnego_gssapi.c
	curl/lib/vauth/vauth.c
	curl/lib/vtls/hostcheck.c
	curl/lib/vtls/keylog.c
	curl/lib/vtls/openssl.c
	curl/lib/vtls/vtls.c
	curl/lib/vtls/x509asn1.c
	curl/lib/vquic/vquic.c
	curl/lib/vquic/vquic-tls.c
	curl/lib/altsvc.c
	curl/lib/asyn-ares.c
	curl/lib/asyn-thread.c
	curl/lib/base64.c
	curl/lib/bufq.c
	curl/lib/bufref.c
	curl/lib/cf-h1-proxy.c
	curl/lib/cf-h2-proxy.c
	curl/lib/cf-haproxy.c
	curl/lib/cf-https-connect.c
	curl/lib/cf-socket.c
	curl/lib/cfilters.c
	curl/lib/conncache.c
	curl/lib/connect.c
	curl/lib/content_encoding.c
	curl/lib/cookie.c
	curl/lib/curl_addrinfo.c
	curl/lib/curl_des.c
	curl/lib/curl_endian.c
	curl/lib/curl_fnmatch.c
	curl/lib/curl_get_line.c
	curl/lib/curl_gethostname.c
	curl/lib/curl_gssapi.c
	curl/lib/curl_memrchr.c
	curl/lib/curl_multibyte.c
	curl/lib/curl_ntlm_core.c
	curl/lib/curl_ntlm_wb.c
	curl/lib/curl_path.c
	curl/lib/curl_range.c
	curl/lib/curl_sasl.c
	curl/lib/curl_sha512_256.c
	curl/lib/curl_threads.c
	curl/lib/curl_trc.c
	curl/lib/cw-out.c
	curl/lib/dict.c
	curl/lib/doh.c
	curl/lib/dynbuf.c
	curl/lib/dynhds.c
	curl/lib/easy.c
	curl/lib/easygetopt.c
	curl/lib/easyoptions.c
	curl/lib/escape.c
	curl/lib/file.c
	curl/lib/fileinfo.c
	curl/lib/fopen.c
	curl/lib/formdata.c
	curl/lib/ftp.c
	curl/lib/ftplistparser.c
	curl/lib/getenv.c
	curl/lib/getinfo.c
	curl/lib/gopher.c
	curl/lib/hash.c
	curl/lib/headers.c
	curl/lib/hmac.c
	curl/lib/hostasyn.c
	curl/lib/hostip.c
	curl/lib/hostip4.c
	curl/lib/hostip6.c
	curl/lib/hostsyn.c
	curl/lib/hsts.c
	curl/lib/http.c
	curl/lib/http1.c
	curl/lib/http2.c
	curl/lib/http_aws_sigv4.c
	curl/lib/http_chunks.c
	curl/lib/http_digest.c
	curl/lib/http_negotiate.c
	curl/lib/http_ntlm.c
	curl/lib/http_proxy.c
	curl/lib/idn.c
	curl/lib/if2ip.c
	curl/lib/imap.c
	curl/lib/inet_ntop.c
	curl/lib/inet_pton.c
	curl/lib/krb5.c
	curl/lib/ldap.c
	curl/lib/llist.c
	curl/lib/macos.c
	curl/lib/md4.c
	curl/lib/md5.c
	curl/lib/memdebug.c
	curl/lib/mime.c
	curl/lib/mprintf.c
	curl/lib/mqtt.c
	curl/lib/multi.c
	curl/lib/netrc.c
	curl/lib/nonblock.c
	curl/lib/noproxy.c
	curl/lib/openldap.c
	curl/lib/parsedate.c
	curl/lib/pingpong.c
	curl/lib/pop3.c
	curl/lib/progress.c
	curl/lib/psl.c
	curl/lib/rand.c
	curl/lib/rename.c
	curl/lib/request.c
	curl/lib/rtsp.c
	curl/lib/select.c
	curl/lib/sendf.c
	curl/lib/setopt.c
	curl/lib/sha256.c
	curl/lib/share.c
	curl/lib/slist.c
	curl/lib/smb.c
	curl/lib/smtp.c
	curl/lib/socketpair.c
	curl/lib/socks.c
	curl/lib/socks_gssapi.c
	curl/lib/socks_sspi.c
	curl/lib/speedcheck.c
	curl/lib/splay.c
	curl/lib/strcase.c
	curl/lib/strdup.c
	curl/lib/strerror.c
	curl/lib/strtok.c
	curl/lib/strtoofft.c
	curl/lib/system_win32.c
	curl/lib/telnet.c
	curl/lib/tftp.c
	curl/lib/timediff.c
	curl/lib/timeval.c
	curl/lib/transfer.c
	curl/lib/url.c
	curl/lib/urlapi.c
	curl/lib/version.c
	curl/lib/warnless.c
	curl/lib/ws.c
)

set(curlexe_SRCS
	curl/lib/base64.c
	curl/lib/curl_multibyte.c
	curl/lib/dynbuf.c
	curl/lib/nonblock.c
	curl/lib/strtoofft.c
	curl/lib/timediff.c
	curl/lib/warnless.c
	curl/src/slist_wc.c
	curl/src/tool_binmode.c
	curl/src/tool_bname.c
	curl/src/tool_cb_dbg.c
	curl/src/tool_cb_hdr.c
	curl/src/tool_cb_prg.c
	curl/src/tool_cb_rea.c
	curl/src/tool_cb_see.c
	curl/src/tool_cb_wrt.c
	curl/src/tool_cfgable.c
	curl/src/tool_dirhie.c
	curl/src/tool_doswin.c
	curl/src/tool_easysrc.c
	curl/src/tool_filetime.c
	curl/src/tool_findfile.c
	curl/src/tool_formparse.c
	curl/src/tool_getparam.c
	curl/src/tool_getpass.c
	curl/src/tool_help.c
	curl/src/tool_helpers.c
	curl/src/tool_hugehelp.c
	curl/src/tool_ipfs.c
	curl/src/tool_libinfo.c
	curl/src/tool_listhelp.c
	curl/src/tool_main.c
	curl/src/tool_msgs.c
	curl/src/tool_operate.c
	curl/src/tool_operhlp.c
	curl/src/tool_paramhlp.c
	curl/src/tool_parsecfg.c
	curl/src/tool_progress.c
	curl/src/tool_setopt.c
	curl/src/tool_sleep.c
	curl/src/tool_stderr.c
	curl/src/tool_strdup.c
	curl/src/tool_urlglob.c
	curl/src/tool_util.c
	curl/src/tool_vms.c
	curl/src/tool_writeout.c
	curl/src/tool_writeout_json.c
	curl/src/tool_xattr.c
	curl/src/var.c
)

include(darling_exe)

# The compatibility and current versions follow Apple's libcurl-version.xcconfig.
set(DYLIB_INSTALL_NAME "/usr/lib/libcurl.4.dylib")
set(DYLIB_COMPAT_VERSION "7.0.0")
set(DYLIB_CURRENT_VERSION "9.0.0")

add_darling_library(curl SHARED ${curl_SRCS})
make_fat(curl)
target_link_libraries(curl PRIVATE system crypto46 ssl48 z nghttp2 CoreFoundation SystemConfiguration Security LDAP Kerberos)
set_target_properties(curl PROPERTIES OUTPUT_NAME "curl.4")

# LIBRESSL_HAS_TLS1_3 makes LibreSSL's headers declare the TLS 1.3 version
# constants, which curl uses to offer TLS 1.3 (--tlsv1.3).
target_compile_definitions(curl PRIVATE
	BUILDING_LIBCURL
	CURL_HIDDEN_SYMBOLS
	LIBRESSL_HAS_TLS1_3
)

# Only the functions marked CURL_EXTERN are exported, as in Apple's build.
target_compile_options(curl PRIVATE -fvisibility=hidden)

if (NOT DARLING_NO_EXECUTABLES)
    add_darling_executable(curlexe ${curlexe_SRCS})
    target_link_libraries(curlexe system curl z)
    set_target_properties(curlexe PROPERTIES OUTPUT_NAME curl)

    install(TARGETS curlexe DESTINATION libexec/darling/usr/bin)
endif (NOT DARLING_NO_EXECUTABLES)

install(TARGETS curl DESTINATION libexec/darling/usr/lib)
InstallSymlink("libcurl.4.dylib" "${CMAKE_INSTALL_PREFIX}/libexec/darling/usr/lib/libcurl.3.dylib")
InstallSymlink("libcurl.4.dylib" "${CMAKE_INSTALL_PREFIX}/libexec/darling/usr/lib/libcurl.dylib")

file(GLOB man1 ${CMAKE_CURRENT_SOURCE_DIR}/curl/docs/curl.1 ${CMAKE_CURRENT_SOURCE_DIR}/curl/docs/curl-config.1)
install(FILES ${man1} DESTINATION libexec/darling/usr/share/man/man1)

file(GLOB man3
	${CMAKE_CURRENT_SOURCE_DIR}/curl/docs/libcurl/*.3
	${CMAKE_CURRENT_SOURCE_DIR}/curl/docs/libcurl/opts/*.3)
install(FILES ${man3} DESTINATION libexec/darling/usr/share/man/man3)
