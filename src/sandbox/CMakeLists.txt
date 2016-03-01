project(sandbox)

set (HELLO_VAR "Hola, soy una variable :D")
message ("Soy un mensaje")
message ("$HELLO_VAR: ${HELLO_VAR}")
message ("$SANDBOX: ${SANDBOX}")
