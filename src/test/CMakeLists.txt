# MPI-driven test programs for lwgrp.
#
# These programs exercise the library's collectives and split operations.
# They do not check results, so a test passes when every rank exits cleanly.

set(LWGRP_TEST_NPROCS 4 CACHE STRING "Number of MPI ranks used to run the lwgrp tests")
if(MPIEXEC_MAX_NUMPROCS AND LWGRP_TEST_NPROCS GREATER MPIEXEC_MAX_NUMPROCS)
  message(WARNING
    "LWGRP_TEST_NPROCS (${LWGRP_TEST_NPROCS}) is larger than MPIEXEC_MAX_NUMPROCS "
    "(${MPIEXEC_MAX_NUMPROCS}); the MPI launcher may refuse to oversubscribe.")
endif()

set(LWGRP_TESTS
  testcommops
  testsplit
  testsplit2
)

foreach(test IN LISTS LWGRP_TESTS)
  add_executable(${test} ${test}.c)
  target_link_libraries(${test} PRIVATE lwgrp)

  add_test(NAME ${test}
    COMMAND ${MPIEXEC_EXECUTABLE} ${MPIEXEC_NUMPROC_FLAG} ${LWGRP_TEST_NPROCS}
            ${MPIEXEC_PREFLAGS} $<TARGET_FILE:${test}> ${MPIEXEC_POSTFLAGS}
  )
  set_tests_properties(${test} PROPERTIES
    PROCESSORS ${LWGRP_TEST_NPROCS}
    TIMEOUT 120
  )
endforeach()
