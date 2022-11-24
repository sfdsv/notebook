```bash
CODE_OUT		:=	../gen-go
SWAG_OUT		:=	../swagger-test/swagger
TEMP			:=	../temp

all:
	mkdir -p ${CODE_OUT}
	mkdir -p ${TEMP}

	protoc -I . \
	--go_out ${CODE_OUT}/ --go_opt paths=source_relative \
	--go-grpc_out ${CODE_OUT}/ --go-grpc_opt paths=source_relative \
	--openapiv2_out ${TEMP}/ --openapiv2_opt logtostderr=true,allow_merge=true \
	--grpc-gateway_out ${CODE_OUT}/ --grpc-gateway_opt logtostderr=true,paths=source_relative,generate_unbound_methods=true \
	*.proto

	cp ${TEMP}/apidocs.swagger.json ${SWAG_OUT}/swagger.json
	rm -rf ${TEMP}

clean:
rm ${CODE_OUT}/*.go
```