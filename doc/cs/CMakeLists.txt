#
# Copyright (C) 2003-2018 Sébastien Helleu <flashcode@flashtux.org>
#
# This file is part of WeeChat, the extensible chat client.
#
# WeeChat is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 3 of the License, or
# (at your option) any later version.
#
# WeeChat is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with WeeChat.  If not, see <http://www.gnu.org/licenses/>.
#

if(ENABLE_MAN)
  # man page
  add_custom_command(
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/weechat.1
    COMMAND ${ASCIIDOCTOR_EXECUTABLE} ARGS ${ASCIIDOCTOR_ARGS} -b manpage -o weechat.1 ${CMAKE_CURRENT_SOURCE_DIR}/weechat.1.cs.adoc
    DEPENDS
    ${CMAKE_CURRENT_SOURCE_DIR}/weechat.1.cs.adoc
    ${CMAKE_CURRENT_SOURCE_DIR}/cmdline_options.cs.adoc
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT "Building weechat.1 (cs)"
  )
  add_custom_target(doc-man-weechat-cs ALL DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/weechat.1)
  install(FILES ${CMAKE_CURRENT_BINARY_DIR}/weechat.1 DESTINATION ${MANDIR}/cs/man1)

  # symbolic link weechat-headless.1 -> weechat.1
  add_custom_command(
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/weechat-headless.1
    COMMAND ${CMAKE_COMMAND} -E remove -f "weechat-headless.1"
    COMMAND ${CMAKE_COMMAND} -E create_symlink "weechat.1" "weechat-headless.1"
    DEPENDS
    ${CMAKE_CURRENT_SOURCE_DIR}/weechat.1.cs.adoc
    ${CMAKE_CURRENT_SOURCE_DIR}/cmdline_options.cs.adoc
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT "Creating symbolic link weechat-headless.1 (cs)"
  )
  add_custom_target(doc-man-weechat-headless-cs ALL DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/weechat-headless.1)
  install(FILES ${CMAKE_CURRENT_BINARY_DIR}/weechat-headless.1 DESTINATION ${MANDIR}/cs/man1)
endif()

if(ENABLE_DOC)

  # quickstart
  add_custom_command(
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/weechat_quickstart.cs.html
    COMMAND ${ASCIIDOCTOR_EXECUTABLE} ARGS ${ASCIIDOCTOR_ARGS} -o weechat_quickstart.cs.html ${CMAKE_CURRENT_SOURCE_DIR}/weechat_quickstart.cs.adoc
    DEPENDS
    ${CMAKE_CURRENT_SOURCE_DIR}/../docinfo.html
    ${CMAKE_CURRENT_SOURCE_DIR}/weechat_quickstart.cs.adoc
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT "Building weechat_quickstart.cs.html"
  )
  add_custom_target(doc-quickstart-cs ALL DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/weechat_quickstart.cs.html)
  install(FILES ${CMAKE_CURRENT_BINARY_DIR}/weechat_quickstart.cs.html DESTINATION ${SHAREDIR}/doc/${PROJECT_NAME})

endif()
