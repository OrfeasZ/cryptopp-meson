project('cryptopp', 'cpp')

include = include_directories('.')

cpp_args = [
	'-DCRYPTOPP_DISABLE_SSSE3',
	'-DCRYPTOPP_DISABLE_SSSE4',
	'-DCRYPTOPP_DISABLE_AESNI',
]

libs = []

sources = [
	'3way.cpp',
	'adler32.cpp',
	#'algebra.cpp',
	'algparam.cpp',
	'arc4.cpp',
	'aria.cpp',
	#'aria-simd.cpp',
	'ariatab.cpp',
	'asn.cpp',
	'authenc.cpp',
	'base32.cpp',
	'base64.cpp',
	'basecode.cpp',
	'bfinit.cpp',
	'blake2.cpp',
	#'blake2-simd.cpp',
	'blowfish.cpp',
	'blumshub.cpp',
	'camellia.cpp',
	'cast.cpp',
	'casts.cpp',
	'cbcmac.cpp',
	'ccm.cpp',
	'chacha.cpp',
	'channels.cpp',
	'cmac.cpp',
	'cpu.cpp',
	'crc.cpp',
	#'crc-simd.cpp',
	'cryptlib.cpp',
	'default.cpp',
	'des.cpp',
	'dessp.cpp',
	'dh.cpp',
	'dh2.cpp',
	'dll.cpp',
	'dsa.cpp',
	'eax.cpp',
	'ec2n.cpp',
	'ecp.cpp',
	'elgamal.cpp',
	'emsa2.cpp',
	'esign.cpp',
	'files.cpp',
	'filters.cpp',
	'fips140.cpp',
	'fipstest.cpp',
	'gcm.cpp',
	#'gcm-simd.cpp',
	'gf256.cpp',
	'gf2_32.cpp',
	'gf2n.cpp',
	'gfpcrypt.cpp',
	'gost.cpp',
	'gzip.cpp',
	'hex.cpp',
	'hmac.cpp',
	'hrtimer.cpp',
	'ida.cpp',
	'idea.cpp',
	'integer.cpp',
	'iterhash.cpp',
	'kalyna.cpp',
	'kalynatab.cpp',
	'keccak.cpp',
	'luc.cpp',
	'mars.cpp',
	'marss.cpp',
	'md2.cpp',
	'md4.cpp',
	'md5.cpp',
	'misc.cpp',
	'modes.cpp',
	'mqueue.cpp',
	'mqv.cpp',
	'nbtheory.cpp',
	'network.cpp',
	'oaep.cpp',
	'osrng.cpp',
	'padlkrng.cpp',
	'panama.cpp',
	'pch.cpp',
	'pkcspad.cpp',
	'poly1305.cpp',
	#'polynomi.cpp',
	'pssr.cpp',
	'pubkey.cpp',
	'queue.cpp',
	'rabin.cpp',
	'randpool.cpp',
	'rc2.cpp',
	'rc5.cpp',
	'rc6.cpp',
	'rdrand.cpp',
	'rdtables.cpp',
	'rijndael.cpp',
	#'rijndael-simd.cpp',
	'ripemd.cpp',
	'rng.cpp',
	'rsa.cpp',
	'rw.cpp',
	'safer.cpp',
	'salsa.cpp',
	'scrypt.cpp',
	'seal.cpp',
	'seed.cpp',
	'serpent.cpp',
	'sha.cpp',
	#'sha-simd.cpp',
	'sha3.cpp',
	'shacal2.cpp',
	#'shacal2-simd.cpp',
	'shark.cpp',
	'sharkbox.cpp',
	'simon.cpp',
	#'simon-simd.cpp',
	#'simple.cpp',
	'skipjack.cpp',
	'sm3.cpp',
	'sm4.cpp',
	'speck.cpp',
	#'speck-simd.cpp',
	'socketft.cpp',
	'sosemanuk.cpp',
	'square.cpp',
	'squaretb.cpp',
	'sse-simd.cpp',
	#'strciphr.cpp',
	'tea.cpp',
	'tftables.cpp',
	'threefish.cpp',
	'tiger.cpp',
	'tigertab.cpp',
	'trdlocal.cpp',
	'ttmac.cpp',
	'tweetnacl.cpp',
	'twofish.cpp',
	'vmac.cpp',
	'wait.cpp',
	'wake.cpp',
	'whrlpool.cpp',
	'winpipes.cpp',
	'xtr.cpp',
	'xtrcrypt.cpp',
	'zdeflate.cpp',
	'zinflate.cpp',
	'zlib.cpp',
]

if get_option('shared_lib')
  libtype = 'shared_library'
  cpp_args += '-DCRYPTOPP_EXPORTS'
else
  libtype = 'static_library'
endif

if target_machine.cpu_family() == 'x86_64'
	cpp_args += [ '-D_M_X64' ]
endif

if target_machine.system() == 'windows'	
	cpp_args += [ '/MTd' ]

	if target_machine.cpu_family() == 'x86_64'
		#gen = generator(executable('ml64.exe'),
		#	output : '@BASENAME@.obj',
		#	arguments : [ '/c', '/nologo', '/D_M_X64', '/W3', '/Cx', '/Zi', '/Fo', '@INPUT@', '@OUTPUT@' ]
		#)

		masm_cmd = [
			[ 'ml64.exe', [ '/c', '/nologo', '/D_M_X64', '/W3', '/Cx', '/Zi', '/Fo', join_paths([ meson.current_source_dir(), 'rdrand-x64.obj' ]), join_paths([ meson.current_source_dir(), 'rdrand.asm' ]) ] ],
			[ 'ml64.exe', [ '/c', '/nologo', '/D_M_X64', '/W3', '/Zi', '/Fo', join_paths([ meson.current_source_dir(), 'x64masm.obj' ]), join_paths([ meson.current_source_dir(), 'x64masm.asm' ]) ] ],
			[ 'ml64.exe', [ '/c', '/nologo', '/D_M_X64', '/W3', '/Zi', '/Fo', join_paths([ meson.current_source_dir(), 'x64dll.obj' ]), join_paths([ meson.current_source_dir(), 'x64dll.asm' ]) ] ]
		]

		libs += [ 'rdrand-x64.obj', 'x64masm.obj', 'x64dll.obj' ]
	else
		masm_cmd = [
			[ 'ml.exe', [ '/c', '/nologo', '/D_M_X86', '/W3', '/Cx', '/Zi', '/safeseh', '/Fo', join_paths([ meson.current_source_dir(), 'rdrand-x86.obj' ]), join_paths([ meson.current_source_dir(), 'rdrand.asm' ]) ] ],
		]

		libs += 'rdrand-x86.obj'
	endif

	foreach cmd : masm_cmd
		res = run_command(cmd[0], cmd[1])

		if res.returncode() != 0
			output = res.stdout().strip()
			errortxt = res.stderr().strip()
			message(output)
			message(errortxt)
			
			error('Assembly compilation failed with error')
		endif
	endforeach
else
	cpp_args += '-msse2'
endif

lib = build_target('cryptopp', sources,
	target_type : libtype,
	cpp_args : cpp_args,
	objects : libs,
	pic : true
)

dep = declare_dependency(
	include_directories : include,
	link_with : [ lib ]
)