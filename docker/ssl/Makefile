CRT_LOCATION = certs
THING_KEY = d7cc2964-a48b-4a6e-871a-08da28e7883d # example value
O = Mainflux
OU = mainflux
EA = info@mainflux.com
CN = localhost
CRT_FILE_NAME = thing

all: clean_certs ca server_crt

# CA name and key is "ca".
ca:
	openssl req -newkey rsa:2048 -x509 -nodes -sha512 -days 1095 \
				-keyout $(CRT_LOCATION)/ca.key -out $(CRT_LOCATION)/ca.crt -subj "/CN=$(CN)/O=$(O)/OU=$(OU)/emailAddress=$(EA)"

# Server cert and key name is "mainflux-server".
server_cert:
	# Create mainflux server key and CSR.
	openssl req -new -sha256 -newkey rsa:4096 -nodes -keyout $(CRT_LOCATION)/mainflux-server.key \
				-out $(CRT_LOCATION)/mainflux-server.csr -subj "/CN=$(CN)/O=$(O)/OU=$(OU)/emailAddress=$(EA)"

	# Sign server CSR.
	openssl x509 -req -days 1000 -in $(CRT_LOCATION)/mainflux-server.csr -CA $(CRT_LOCATION)/ca.crt -CAkey $(CRT_LOCATION)/ca.key -CAcreateserial -out $(CRT_LOCATION)/mainflux-server.crt

	# Remove CSR.
	rm $(CRT_LOCATION)/mainflux-server.csr

thing_cert:
	# Create mainflux server key and CSR.
	openssl req -new -sha256 -newkey rsa:4096 -nodes -keyout $(CRT_LOCATION)/$(CRT_FILE_NAME).key \
				-out $(CRT_LOCATION)/$(CRT_FILE_NAME).csr -subj "/CN=$(THING_KEY)/O=$(O)/OU=$(OU)/emailAddress=$(EA)"

	# Sign client CSR.
	openssl x509 -req -days 730 -in $(CRT_LOCATION)/$(CRT_FILE_NAME).csr -CA $(CRT_LOCATION)/ca.crt -CAkey $(CRT_LOCATION)/ca.key -CAcreateserial -out $(CRT_LOCATION)/$(CRT_FILE_NAME).crt

	# Remove CSR.
	rm $(CRT_LOCATION)/$(CRT_FILE_NAME).csr

clean_certs:
	rm -r $(CRT_LOCATION)/*.crt
	rm -r $(CRT_LOCATION)/*.key
	rm -r $(CRT_LOCATION)/*.srl
