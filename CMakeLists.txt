project(libcurl)

cmake_minimum_required(VERSION 2.4.0)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
	cmake_policy(SET CMP0005 NEW)
endif(COMMAND cmake_policy)

add_definitions(-D__APPLE__ -D__MACH__)
add_definitions(-DTARGET_OS_MAC=1)
add_definitions(-DHAVE_STDINT_H=1)
add_definitions(-D__APPLE__ -D__DYNAMIC__)
add_definitions(-D__ENVIRONMENT_MAC_OS_X_VERSION_MIN_REQUIRED__=1080)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -nostdinc -D__DARWIN_UNIX03 -fPIC -w")
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -nostdlib -Wl,--version-script=${DARLING_TOP_DIRECTORY}/darwin.map")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -nostdlib -Wl,--version-script=${DARLING_TOP_DIRECTORY}/darwin.map")

SET(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}/darling")
SET(CMAKE_BUILD_WITH_INSTALL_RPATH TRUE) 
SET(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/curl/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/curl/lib)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../openssl/src/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../zlib)

add_definitions(-DHAVE_CONFIG_H=1)
add_definitions(-DOS="Darling")
add_definitions(-DCLIENTNAME="libcurl")

set(curl_SRCS
	curl/lib/asyn-ares.c
	curl/lib/asyn-thread.c
	curl/lib/base64.c
	curl/lib/conncache.c
	curl/lib/connect.c
	curl/lib/content_encoding.c
	curl/lib/cookie.c
	curl/lib/curl_addrinfo.c
	curl/lib/curl_des.c
	curl/lib/curl_endian.c
	curl/lib/curl_fnmatch.c
	curl/lib/curl_gethostname.c
	curl/lib/curl_gssapi.c
	curl/lib/curl_memrchr.c
	curl/lib/curl_multibyte.c
	curl/lib/curl_ntlm.c
	curl/lib/curl_ntlm_core.c
	curl/lib/curl_ntlm_msgs.c
	curl/lib/curl_ntlm_wb.c
	curl/lib/curl_rtmp.c
	curl/lib/curl_sasl.c
	curl/lib/curl_sasl_gssapi.c
	curl/lib/curl_sasl_sspi.c
	curl/lib/curl_sspi.c
	curl/lib/curl_threads.c
	curl/lib/dict.c
	curl/lib/dotdot.c
	curl/lib/easy.c
	curl/lib/escape.c
	curl/lib/file.c
	curl/lib/fileinfo.c
	curl/lib/formdata.c
	curl/lib/ftp.c
	curl/lib/ftplistparser.c
	curl/lib/getenv.c
	curl/lib/getinfo.c
	curl/lib/gopher.c
	curl/lib/hash.c
	curl/lib/hmac.c
	curl/lib/hostasyn.c
	curl/lib/hostcheck.c
	curl/lib/hostip4.c
	curl/lib/hostip6.c
	curl/lib/hostip.c
	curl/lib/hostsyn.c
	curl/lib/http2.c
	curl/lib/http.c
	curl/lib/http_chunks.c
	curl/lib/http_digest.c
	curl/lib/http_negotiate.c
	curl/lib/http_negotiate_sspi.c
	curl/lib/http_proxy.c
	curl/lib/idn_win32.c
	curl/lib/if2ip.c
	curl/lib/imap.c
	curl/lib/inet_ntop.c
	curl/lib/inet_pton.c
	curl/lib/krb5.c
	curl/lib/ldap.c
	curl/lib/llist.c
	curl/lib/md4.c
	curl/lib/md5.c
	curl/lib/memdebug.c
	curl/lib/mprintf.c
	curl/lib/multi.c
	curl/lib/netrc.c
	curl/lib/non-ascii.c
	curl/lib/nonblock.c
	curl/lib/nwlib.c
	curl/lib/nwos.c
	curl/lib/openldap.c
	curl/lib/parsedate.c
	curl/lib/pingpong.c
	curl/lib/pipeline.c
	curl/lib/pop3.c
	curl/lib/progress.c
	curl/lib/rawstr.c
	curl/lib/rtsp.c
	curl/lib/security.c
	curl/lib/select.c
	curl/lib/sendf.c
	curl/lib/share.c
	curl/lib/slist.c
	curl/lib/smb.c
	curl/lib/smtp.c
	curl/lib/socks.c
	curl/lib/socks_gssapi.c
	curl/lib/socks_sspi.c
	curl/lib/speedcheck.c
	curl/lib/splay.c
	curl/lib/ssh.c
	curl/lib/strdup.c
	curl/lib/strequal.c
	curl/lib/strerror.c
	curl/lib/strtok.c
	curl/lib/strtoofft.c
	curl/lib/telnet.c
	curl/lib/tftp.c
	curl/lib/timeval.c
	curl/lib/transfer.c
	curl/lib/url.c
	curl/lib/version.c
	curl/lib/warnless.c
	curl/lib/wildcard.c
	curl/lib/x509asn1.c
	curl/lib/vtls/openssl.c
	curl/lib/vtls/vtls.c
)

set(curlexe_SRCS
	curl/src/tool_binmode.c
	curl/src/tool_bname.c
	curl/src/tool_cb_dbg.c
	curl/src/tool_cb_hdr.c
	curl/src/tool_cb_prg.c
	curl/src/tool_cb_rea.c
	curl/src/tool_cb_see.c
	curl/src/tool_cb_wrt.c
	curl/src/tool_cfgable.c
	curl/src/tool_convert.c
	curl/src/tool_dirhie.c
	curl/src/tool_doswin.c
	curl/src/tool_easysrc.c
	curl/src/tool_formparse.c
	curl/src/tool_getparam.c
	curl/src/tool_getpass.c
	curl/src/tool_help.c
	curl/src/tool_helpers.c
	curl/src/tool_homedir.c
	curl/src/tool_hugehelp.c
	curl/src/tool_libinfo.c
	curl/src/tool_main.c
	curl/src/tool_metalink.c
	curl/src/tool_mfiles.c
	curl/src/tool_msgs.c
	curl/src/tool_operate.c
	curl/src/tool_operhlp.c
	curl/src/tool_panykey.c
	curl/src/tool_paramhlp.c
	curl/src/tool_parsecfg.c
	curl/src/tool_setopt.c
	curl/src/tool_sleep.c
	curl/src/tool_strdup.c
	curl/src/tool_urlglob.c
	curl/src/tool_util.c
	# curl/src/tool_vms.c
	curl/src/tool_writeenv.c
	curl/src/tool_writeout.c
	curl/src/tool_xattr.c
)

include(darling_exe)

add_library(curl SHARED ${curl_SRCS})
target_link_libraries(curl PRIVATE system ssl098 z)

if (NOT DARLING_NO_EXECUTABLES)
    add_darling_executable(curlexe ${curlexe_SRCS})
    target_link_libraries(curlexe curl)
    set_target_properties(curlexe PROPERTIES OUTPUT_NAME curl)
    
    install(TARGETS curlexe DESTINATION libexec/darling/usr/bin)
endif (NOT DARLING_NO_EXECUTABLES)

install(TARGETS curl DESTINATION ${CMAKE_INSTALL_LIBDIR}/darling)

