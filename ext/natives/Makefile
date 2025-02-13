GTA_INPUTS=$(wildcard natives_stash/gta_*.lua)

GTA_OUTPUTS_LUA=$(GTA_INPUTS:natives_stash/gta_%=out/natives_%)
GTA_OUTPUTS_JS=$(GTA_OUTPUTS_LUA:.lua=.js)
GTA_OUTPUTS_ZIP=$(GTA_OUTPUTS_LUA:.lua=.zip)
GTA_OUTPUTS_DTS=$(GTA_OUTPUTS_LUA:.lua=.d.ts)

RDR_INPUTS=$(wildcard natives_stash/rdr3_*.lua)

RDR_OUTPUTS_LUA=$(RDR_INPUTS:natives_stash/rdr3_%=out/rdr3_%)
RDR_OUTPUTS_JS=$(RDR_OUTPUTS_LUA:.lua=.js)
RDR_OUTPUTS_ZIP=$(RDR_OUTPUTS_LUA:.lua=.zip)
RDR_OUTPUTS_DTS=$(RDR_OUTPUTS_LUA:.lua=.d.ts)

NY_INPUTS=$(wildcard natives_stash/ny_*.lua)

NY_OUTPUTS_LUA=$(NY_INPUTS:natives_stash/ny_%=out/ny_%)
NY_OUTPUTS_JS=$(NY_OUTPUTS_LUA:.lua=.js)
NY_OUTPUTS_ZIP=$(NY_OUTPUTS_LUA:.lua=.zip)
NY_OUTPUTS_DTS=$(NY_OUTPUTS_LUA:.lua=.d.ts)

CODEGEN_SCRIPTS=codegen.lua codegen_types.lua markdown.lua inp/natives_cfx.lua

define NATIVES_FIVE_HEADER
#if GTA_FIVE
#if USE_HYPERDRIVE
using ContextType = CitizenFX.Core.RageScriptContext;
#else
using ContextType = CitizenFX.Core.fxScriptContext;
#endif

namespace CitizenFX.Core.Native
{
endef

define NATIVES_FIVE_FOOTER
}
#endif
endef

export NATIVES_FIVE_HEADER
export NATIVES_FIVE_FOOTER

define NATIVES_SERVER_HEADER
#if IS_FXSERVER
using ContextType = CitizenFX.Core.fxScriptContext;

namespace CitizenFX.Core.Native
{
endef

define NATIVES_SERVER_FOOTER
}
#endif
endef

export NATIVES_SERVER_HEADER
export NATIVES_SERVER_FOOTER

define NATIVES_RDR3_HEADER
#if IS_RDR3
#if USE_HYPERDRIVE
using ContextType = CitizenFX.Core.RageScriptContext;
#else
using ContextType = CitizenFX.Core.fxScriptContext;
#endif

namespace CitizenFX.Core.Native
{
endef

define NATIVES_RDR3_FOOTER
}
#endif
endef

export NATIVES_RDR3_HEADER
export NATIVES_RDR3_FOOTER

define NATIVES_NY_HEADER
#if GTA_NY
#if USE_HYPERDRIVE
using ContextType = CitizenFX.Core.RageScriptContext;
#else
using ContextType = CitizenFX.Core.fxScriptContext;
#endif

namespace CitizenFX.Core.Native
{
endef

define NATIVES_NY_FOOTER
}
#endif
endef

export NATIVES_NY_HEADER
export NATIVES_NY_FOOTER

all: out \
	out/NativesFive.cs out/NativesServer.cs out/NativesRDR3.cs out/NativesNY.cs \
	out/rpc_natives.json \
	out/natives_server.lua out/natives_server.d.ts out/natives_server.js out/natives_server.zip \
	out/Natives.h out/NativesRDR.h out/NativesServer.h out/NativesNY.h \
	$(GTA_OUTPUTS_LUA) $(GTA_OUTPUTS_JS) $(GTA_OUTPUTS_DTS) $(GTA_OUTPUTS_ZIP) \
	$(RDR_OUTPUTS_LUA) $(RDR_OUTPUTS_JS) $(RDR_OUTPUTS_DTS) $(RDR_OUTPUTS_ZIP) \
	$(NY_OUTPUTS_LUA) $(NY_OUTPUTS_JS) $(NY_OUTPUTS_DTS) $(NY_OUTPUTS_ZIP)

.PHONY: clean
clean:
	rm -r out

out:
	mkdir -p out

out/NativesFive.cs: inp/natives_global.lua $(CODEGEN_SCRIPTS) codegen_out_cs.lua codegen_out_enum.lua
	echo "$$NATIVES_FIVE_HEADER" > $@
	./lua53 codegen.lua inp/natives_global.lua enum >> $@
	./lua53 codegen.lua inp/natives_global.lua cs >> $@
	echo "$$NATIVES_FIVE_FOOTER" >> $@

out/NativesRDR3.cs: inp/natives_rdr3.lua $(CODEGEN_SCRIPTS) codegen_out_cs.lua codegen_out_enum.lua
	echo "$$NATIVES_RDR3_HEADER" > $@
	./lua53 codegen.lua inp/natives_rdr3.lua enum >> $@
	./lua53 codegen.lua inp/natives_rdr3.lua cs >> $@
	echo "$$NATIVES_RDR3_FOOTER" >> $@

out/NativesNY.cs: inp/natives_ny.lua $(CODEGEN_SCRIPTS) codegen_out_cs.lua codegen_out_enum.lua
	echo "$$NATIVES_NY_HEADER" > $@
	./lua53 codegen.lua inp/natives_ny.lua enum >> $@
	./lua53 codegen.lua inp/natives_ny.lua cs >> $@
	echo "$$NATIVES_NY_FOOTER" >> $@

out/NativesServer.cs: $(CODEGEN_SCRIPTS) inp/natives_global.lua codegen_out_cs.lua rpc_spec_natives.lua
	echo "$$NATIVES_SERVER_HEADER" > $@
	./lua53 codegen.lua inp/natives_global.lua enum server >> $@
	./lua53 codegen.lua inp/natives_global.lua cs server >> $@
	echo "$$NATIVES_SERVER_FOOTER" >> $@

out/Natives.h: inp/natives_global.lua $(CODEGEN_SCRIPTS) codegen_out_native_lua.lua rpc_spec_natives.lua
	./lua53 codegen.lua inp/natives_global.lua native_lua > $@

out/NativesRDR.h: inp/natives_rdr3.lua $(CODEGEN_SCRIPTS) codegen_out_native_lua.lua rpc_spec_natives.lua
	./lua53 codegen.lua inp/natives_rdr3.lua native_lua > $@

out/NativesNY.h: inp/natives_ny.lua $(CODEGEN_SCRIPTS) codegen_out_native_lua.lua rpc_spec_natives.lua
	./lua53 codegen.lua inp/natives_ny.lua native_lua > $@

out/NativesServer.h: inp/natives_global.lua $(CODEGEN_SCRIPTS) codegen_out_native_lua.lua rpc_spec_natives.lua
	./lua53 codegen.lua inp/natives_global.lua native_lua server > $@

out/rpc_natives.json: $(CODEGEN_SCRIPTS) inp/natives_global.lua codegen_out_rpc.lua rpc_spec_natives.lua
	./lua53 codegen.lua inp/natives_global.lua rpc server > $@

out/natives_server.lua: $(CODEGEN_SCRIPTS) inp/natives_global.lua codegen_out_lua.lua rpc_spec_natives.lua
	./lua53 codegen.lua inp/natives_global.lua lua server > $@

out/natives_server.d.ts: $(CODEGEN_SCRIPTS) inp/natives_global.lua codegen_out_dts.lua rpc_spec_natives.lua
	./lua53 codegen.lua inp/natives_global.lua dts server > $@

out/natives_server.js: $(CODEGEN_SCRIPTS) inp/natives_global.lua codegen_out_js.lua rpc_spec_natives.lua
	./lua53 codegen.lua inp/natives_global.lua js server > $@

out/natives_server.zip: inp/natives_global.lua $(CODEGEN_SCRIPTS) codegen_out_lua.lua codegen_out_slua.lua  rpc_spec_natives.lua
	$(eval DIR := $(shell mktemp -d -p out/))
	rm $@ || true
	NATIVES_DIR="$(DIR)" ./lua53 codegen.lua inp/natives_global.lua slua server > /dev/null
	sh -c 'pushd $(DIR) && ../../../../code/tools/ci/7z.exe a -mx=0 ../../$@ \*.lua && popd'
	rm -r $(DIR) || true

out/natives_%.lua: inp/natives_global.lua inp/natives_rdr3.lua $(CODEGEN_SCRIPTS) codegen_out_lua.lua
	./lua53 codegen.lua $< lua > $@

out/natives_%.d.ts: inp/natives_global.lua $(CODEGEN_SCRIPTS) codegen_out_dts.lua
	./lua53 codegen.lua $< dts > $@

out/natives_%.js: inp/natives_global.lua $(CODEGEN_SCRIPTS) codegen_out_js.lua
	./lua53 codegen.lua $< js > $@

out/natives_%.zip: natives_stash/gta_%.lua inp/natives_global.lua $(CODEGEN_SCRIPTS) codegen_out_lua.lua codegen_out_slua.lua
	$(eval DIR := $(shell mktemp -d -p out/))
	rm $@ || true
	NATIVES_DIR="$(DIR)" ./lua53 codegen.lua $< slua > /dev/null
	sh -c 'pushd $(DIR) && ../../../../code/tools/ci/7z.exe a -mx=0 ../../$@ \*.lua && popd'
	rm -r $(DIR) || true

out/rdr3_%.lua: inp/natives_rdr3.lua $(CODEGEN_SCRIPTS) codegen_out_lua.lua
	./lua53 codegen.lua $< lua > $@

out/rdr3_%.d.ts: inp/natives_rdr3.lua $(CODEGEN_SCRIPTS) codegen_out_dts.lua
	./lua53 codegen.lua $< dts > $@

out/rdr3_%.js: inp/natives_rdr3.lua $(CODEGEN_SCRIPTS) codegen_out_js.lua
	./lua53 codegen.lua $< js > $@

out/rdr3_%.zip: natives_stash/rdr3_%.lua inp/natives_rdr3.lua $(CODEGEN_SCRIPTS) codegen_out_lua.lua codegen_out_slua.lua
	$(eval DIR := $(shell mktemp -d -p out/))
	rm $@ || true
	NATIVES_DIR="$(DIR)" ./lua53 codegen.lua $< slua > /dev/null
	sh -c 'pushd $(DIR) && ../../../../code/tools/ci/7z.exe a -mx=0 ../../$@ \*.lua && popd'
	rm -r $(DIR) || true

out/ny_%.lua: inp/natives_ny.lua $(CODEGEN_SCRIPTS) codegen_out_lua.lua
	./lua53 codegen.lua $< lua > $@

out/ny_%.d.ts: inp/natives_ny.lua $(CODEGEN_SCRIPTS) codegen_out_dts.lua
	./lua53 codegen.lua $< dts > $@

out/ny_%.js: inp/natives_ny.lua $(CODEGEN_SCRIPTS) codegen_out_js.lua
	./lua53 codegen.lua $< js > $@

out/ny_%.zip: natives_stash/ny_%.lua inp/natives_ny.lua $(CODEGEN_SCRIPTS) codegen_out_lua.lua codegen_out_slua.lua
	$(eval DIR := $(shell mktemp -d -p out/))
	rm $@ || true
	NATIVES_DIR="$(DIR)" ./lua53 codegen.lua $< slua > /dev/null
	sh -c 'pushd $(DIR) && ../../../../code/tools/ci/7z.exe a -mx=0 ../../$@ \*.lua && popd'
	rm -r $(DIR) || true