## Known Issues
1. You might see 
```
# runtime
../../Tools/go/src/runtime/asm_wasm.s:373: unrecognized instruction "MemoryCopy"
```

When you compile go files via new go. That is related to cache invalidation, presumably.

a. First, make sure you are using customized binary of go rather than the default one which is a release from official website;

b. Try not to use wasm as target, ${CUSTOMIZED_GO_PATH}/bin/go build example.go

If 2 goes well

c. env GO111MODULE=on GOOS=wasip1 GOARCH=wasm ${CUSTOMIZED_GO_PATH}/bin/go build -gcflags=all=-d=softfloat -o example.wasm example.go


## Comparison
%go version
go version go1.21.1 darwin/arm64

% ${CUSTOMIZED}/Tools/go/bin/go version
go version devel go1.22-06c4df495c Sun Sep 10 16:52:37 2023 -0700 darwin/arm64

%env GO111MODULE=on GOOS=wasip1 GOARCH=wasm ${CUSTOMIZED}/Tools/go/bin/go build -gcflags=all=-d=softfloat -o mem-wasi-op-nobulk.wasm mem.go
%env GO111MODULE=on GOOS=wasip1 GOARCH=wasm go build -gcflags=all=-d=softfloat -o mem-wasi.wasm mem.go 

%wasm2wat mem-wasi-op-nobulk.wasm > mem-wasi-op-nobulk.wat 
% wasm2wat mem-wasi.wasm > mem-wasi.wat

% grep "memory.fill" mem-wasi.wat | wc -l
      52
% grep "memory.fill" mem-wasi-op-nobulk.wat | wc -l
       0



