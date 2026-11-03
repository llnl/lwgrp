add_library(lwgrp)
add_library(lwgrp::lwgrp ALIAS lwgrp)

target_sources(lwgrp
PRIVATE
  lwgrp.c
  lwgrp_util.c
  lwgrp_chain_ops.c
  lwgrp_ring_ops.c
  lwgrp_logchain_ops.c
  lwgrp_logring_ops.c
  lwgrp_comm.c
  lwgrp_comm_split.c

FILE_SET HEADERS
  PUBLIC
  FILES
    lwgrp.h

FILE_SET private_headers
  TYPE HEADERS
  PRIVATE 
  FILES
    lwgrp_internal.h
)
if(USE_MPIANYSOURCE)
  target_compile_definitions(lwgrp PUBLIC LWGRP_USE_ANYSOURCE)
endif()

target_link_libraries(lwgrp PUBLIC MPI::MPI_C)

if(LWGRP_ENABLE_TESTS)
  add_subdirectory(test)
endif()
