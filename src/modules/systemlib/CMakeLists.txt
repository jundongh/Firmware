############################################################################
#
#   Copyright (c) 2015 PX4 Development Team. All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions
# are met:
#
# 1. Redistributions of source code must retain the above copyright
#    notice, this list of conditions and the following disclaimer.
# 2. Redistributions in binary form must reproduce the above copyright
#    notice, this list of conditions and the following disclaimer in
#    the documentation and/or other materials provided with the
#    distribution.
# 3. Neither the name PX4 nor the names of its contributors may be
#    used to endorse or promote products derived from this software
#    without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
# "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
# LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS
# FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE
# COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT,
# INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,
# BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS
# OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED
# AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
# LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN
# ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
# POSSIBILITY OF SUCH DAMAGE.
#
############################################################################

set(SRCS
	airspeed.cpp
	board_serial.c
	circuit_breaker.cpp
	conversions.c
	cpuload.c
	crc.c
	hysteresis/hysteresis.cpp
	mavlink_log.c
	otp.c
	pid/pid.c
	pwm_limit/pwm_limit.c
	rc_check.c
	)

if(${OS} STREQUAL "nuttx")
	list(APPEND SRCS
		err.c
		printload.c
		)
elseif ("${CONFIG_SHMEM}" STREQUAL "1")
	list(APPEND SRCS
		print_load_posix.c
		)
else()
	list(APPEND SRCS
		print_load_posix.c
		)
endif()

# TODO: find a better way to do this
if (NOT "${CONFIG}" MATCHES "px4io")
	px4_add_library(systemlib ${SRCS})
	target_compile_options(systemlib PRIVATE -Wno-sign-compare)
else()
	add_library(systemlib ${PX4_SOURCE_DIR}/src/platforms/empty.c)
	add_dependencies(systemlib prebuild_targets)
endif()
