#add doxygen target to extract documentation from source
find_package(Doxygen)
if(DOXYGEN_FOUND)
	configure_file(${CMAKE_CURRENT_SOURCE_DIR}/Doxyfile.in ${CMAKE_CURRENT_BINARY_DIR}/Doxyfile @ONLY)

	add_custom_target(doc ${DOXYGEN_EXECUTABLE} ${CMAKE_CURRENT_BINARY_DIR}/Doxyfile  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
	COMMENT "Extracting Frommle documentation using Doxygen" VERBATIM
)

	
###Cosmetic tweaks to add a favicon to the header. This section may be commented out when it give rise to problems
	add_custom_command(OUTPUT header.html COMMAND ${DOXYGEN_EXECUTABLE} -w html header.html footer.html stylesheet.html
	COMMAND perl -p -i -e 's/^HTML_HEADER.+/HTML_HEADER\ \ \ \ \ \ \ \ \ \ \ \ = header.html/' Doxyfile
	) 
#	
	add_custom_target(favicon 
	perl -p -i -f ${CMAKE_CURRENT_SOURCE_DIR}/addfavicon.pl header.html DEPENDS header.html
	WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR} COMMENT "adding favicon to doxygen header" VERBATIM) 

    add_dependencies(doc favicon)

###end of cosmetic tweaks
	
	
	



else(DOXYGEN_FOUND)
	MESSAGE("Skipping Doxygen Document generation")
endif(DOXYGEN_FOUND)
