# Table of Contents  
1. [EmPOWER REST API](#restapi)  
2. [API Resources](#api)

<a name="restapi"/>

## EmPOWER REST API

The EmPOWER API consists of a set of RESTful resources and their attributes. The base URL for the EmPOWER REST API is the following:

    http{s}://{username}:{password}@{hostname}:{port}/api/v1/{resource}

Of course you need to replace hostname and port with the hostname/port combination for your controller.

The current (and only) version of the API is v1. EmPOWER uses HTTP basic authentication in order to control and limit access to RESTful resource. Notice that there are two kinds of accounts:

root account, which allow complete access to all resources
user account, which allow access only to the resources associated to a virtual network
All the URLs that do not start with /api/v1/tenants/{resource} require a root account.

<a name="api"/>


### AccountsHandler
Accounts handler. Used to add/remove accounts.
#### URLs
    /api/v1/accounts/?
    /api/v1/accounts/([a-zA-Z0-9:.]*)/?
#### GET
List the accounts.

Args:

    [0]: the username

Example URLs:

    GET /api/v1/accounts
    GET /api/v1/accounts/root
### AllowHandler
Allow handler. Used to add/remove allowed Wi-Fi clients.
#### URLs
    /api/v1/allow/?
    /api/v1/allow/([a-zA-Z0-9:]*)/?
#### GET
List the allowed Wi-Fi clients.

Args:

    [0]: the station address

Example URLs:

    GET /api/v1/allow
    GET /api/v1/allow/11:22:33:44:55:66
### AuthLoginHandler
Login handler.
#### URLs
    /auth/login
### AuthLogoutHandler
Logout handler.
#### URLs
    /auth/logout
### BaseHandler
Base handler.
### CPPHandler
CPP Handler. Used to view and manipulate CPPs.

Used to view and manipulate CPPs. This handler has access to the system wide CPPs Notice how a CPP is a subclass of PNFDev.
#### URLs
    /api/v1/cpps/?
    /api/v1/cpps/([a-zA-Z0-9:]*)/?
#### GET
List PNFDevs.

Args:

    [0]: the pnfdev address

Example URLs:

    GET /api/v1/<wtps|cpps|vbses>
    GET /api/v1/<wtps|cpps|vbses>/11:22:33:44:55:66
### ComponentsHandler
Components handler. Used to load/unload components.
#### URLs
    /api/v1/components/?
    /api/v1/components/([a-zA-Z0-9:_\-.]*)/?
#### GET
Lists components.

Args:

    [0]: the component id (optional)

Example URLs:

    GET /api/v1/components
    GET /api/v1/components/empower.apps.mobilitymanager.mobilitymanager
### DocHandler
Generates MD documentation.
#### URLs
    /api/v1/doc/?
#### GET
Generates MD documentation.

Args:
    None

Example URLs:
    GET /api/v1/doc
### IndexHandler
Index page
#### URLs
    /
    /index.html
#### GET
Render page. 
### LVAPHandler
LVAP handler. Used to view LVAPs (controller-wide).
#### URLs
    /api/v1/lvaps/?
    /api/v1/lvaps/([a-zA-Z0-9:]*)/?
#### GET
Get all LVAPs or just the specified one.

Args:
    lvap_id: the lvap address

Example URLs:
    GET /api/v1/lvaps
    GET /api/v1/lvaps/11:22:33:44:55:66
### ModuleHandler
Tenat traffic rule queue handler.
#### URLs
    /api/v1/tenants/([a-zA-Z0-9:-]*)/modules/([a-zA-Z_.]*)/?
    /api/v1/tenants/([a-zA-Z0-9:-]*)/modules/([a-zA-Z_.]*)/([0-9]*)/?
#### GET
List traffic rules .

Args:

    tenant_id: network name of a tenant
    module_name: the name of the module
    module_id: the id of the module

Example URLs:

    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/wifi_stats
### SliceHandler
Slice handler. Used to view slices.
#### URLs
    /api/v1/slices
#### GET
Lists all the slices managed by this controller.

Args:
    None

Example URLs:
    GET /api/v1/slices
### TenantCPPHandler
TenantCPPHandler Handler.

Used to view and manipulate CPPs. This handler has access to the Tenant CPPs Notice how a CPP is a subclass of PNFDev.
#### URLs
    /api/v1/tenants/([a-zA-Z0-9-]*)/cpps/?
    /api/v1/tenants/([a-zA-Z0-9-]*)/cpps/([a-zA-Z0-9:]*)/?
#### GET
List PNFDevs.

Args:

    [0]: the tenant id
    [1]: the pnfdev address

Example URLs:

    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/
      <wtps|cpps|vbses>
    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/
      <wtps|cpps|vbses>/11:22:33:44:55:66
### TenantComponentsHandler
Components handler. Used to load/unload components.
#### URLs
    /api/v1/tenants/([a-zA-Z0-9-]*)/components/?
    /api/v1/tenants/([a-zA-Z0-9-]*)/components/([a-zA-Z0-9:\-.]*)/?
#### GET
Lists all the components running in a tenant.

Args:
    [0]: the tenant id
    [0]: the component id

Example URLs:
    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26
    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/<id>
### TenantEndpointHandler
TenantEndpointHandler Handler.
#### URLs
    /api/v1/tenants/([a-zA-Z0-9-]*)/eps/?
    /api/v1/tenants/([a-zA-Z0-9-]*)/eps/([a-zA-Z0-9-]*)/?
#### GET
List all Functions.

Args:
    tenant_id: the network names of the tenant
    endpoint_id: the endpoint uuid

Example URLs:

    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/eps
    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/
        eps/49313ecb-9d00-4a7c-b873-b55d3d9ada34
### TenantEndpointNextHandler
Tenant/Endpoint/Port/Next Handler.
#### URLs
    /api/v1/tenants/([a-zA-Z0-9-]*)/eps/([a-zA-Z0-9-]*)/ports/([0-9]*)/next/?
    /api/v1/tenants/([a-zA-Z0-9-]*)/eps/([a-zA-Z0-9-]*)/ports/([0-9]*)/next/([a-zA-Z0-9_:,=]*)/?
#### GET
List next associations.

Args:
    [0]: the tenant id
    [1]: the endpoint id
    [2]: the port id
    [3]: match

Example URLs:

    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/
        eps/49313ecb-9d00-4a7c-b873-b55d3d9ada34/ports/1/next

    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/
        eps/49313ecb-9d00-4a7c-b873-b55d3d9ada34/ports/1/next/
        in_port=1,dl_type=800,nw_proto=84
### TenantEndpointPortHandler
Tenant/Endpoint/Port Handler.
#### URLs
    /api/v1/tenants/([a-zA-Z0-9-]*)/eps/([a-zA-Z0-9-]*)/ports/?
    /api/v1/tenants/([a-zA-Z0-9-]*)/eps/([a-zA-Z0-9-]*)/ports/([0-9]*)/?
#### GET
List all ports.

Args:
    [0]: the tenant id
    [1]: the endpoint id
    [2]: the port id

Example URLs:

    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/
        eps/49313ecb-9d00-4a7c-b873-b55d3d9ada34/ports

    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/
        eps/49313ecb-9d00-4a7c-b873-b55d3d9ada34/ports/1
### TenantHandler
Tenat handler. Used to view and manipulate tenants.
#### URLs
    /api/v1/tenants/?
    /api/v1/tenants/([a-zA-Z0-9-]*)/?
#### GET
Lists all the tenants managed by this controller.

Args:
    [0]: network name of a tenant

Example URLs:
    GET /api/v1/tenants
    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26
### TenantLVAPHandler
TenantLVAP handler. Used to view and manipulate LVAPs in tenants.
#### URLs
    /api/v1/tenants/([a-zA-Z0-9-]*)/lvaps/?
    /api/v1/tenants/([a-zA-Z0-9-]*)/lvaps/([a-zA-Z0-9:]*)/?
#### GET
Get all LVAPs in a Pool or just the specified one.

Args:
    pool_id: the network name
    lvap_id: the lvap address

Example URLs:
    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/lvaps
    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/
      lvaps/11:22:33:44:55:66
### TenantLVNFHandler
Tenant Function Handler. Used to view anc manipulate Functions.
#### URLs
    /api/v1/tenants/([a-zA-Z0-9-]*)/lvnfs/?
    /api/v1/tenants/([a-zA-Z0-9-]*)/lvnfs/([a-zA-Z0-9-]*)/?
#### GET
List all Functions.

Args:
    tenant_id: the network names of the tenant
    lvnf_id: the address of the cpp

Example URLs:

    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/lvnfs
    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/
        lvnfs/49313ecb-9d00-4a7c-b873-b55d3d9ada34
### TenantLVNFNextHandler
Tenant/LVNF/Port/Next Handler.
#### URLs
    /api/v1/tenants/([a-zA-Z0-9-]*)/lvnfs/([a-zA-Z0-9-]*)/ports/([0-9]*)/next/?
    /api/v1/tenants/([a-zA-Z0-9-]*)/lvnfs/([a-zA-Z0-9-]*)/ports/([0-9]*)/next/([a-zA-Z0-9_:,=]*)/?
#### GET
List next associations.

Args:
    [0]: the tenant id
    [1]: the lvnf id
    [2]: the port id

Example URLs:

    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/
        lvnfs/49313ecb-9d00-4a7c-b873-b55d3d9ada34/ports/1/next

    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/
        lvnfs/49313ecb-9d00-4a7c-b873-b55d3d9ada34/ports/1/next/
        in_port=1,dl_type=800,nw_proto=84
### TenantLVNFPortHandler
Tenant/LVNF/Port Handler.
#### URLs
    /api/v1/tenants/([a-zA-Z0-9-]*)/lvnfs/([a-zA-Z0-9-]*)/ports/?
    /api/v1/tenants/([a-zA-Z0-9-]*)/lvnfs/([a-zA-Z0-9-]*)/ports/([0-9]*)/?
#### GET
List all ports.

Args:
    [0]: the tenant id
    [1]: the lvap id
    [2]: the port id

Example URLs:

    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/
        lvnfs/49313ecb-9d00-4a7c-b873-b55d3d9ada34/ports

    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/
        lvnfs/49313ecb-9d00-4a7c-b873-b55d3d9ada34/ports/1
### TenantSliceHandler
Tenat slice handler.
#### URLs
    /api/v1/tenants/([a-zA-Z0-9-]*)/slices/?
    /api/v1/tenants/([a-zA-Z0-9-]*)/slices/([a-zA-Z0-9-]*)/?
#### GET
List slices.

Args:
    tenant_id: network name of a tenant
    dscp: the slice DSCP

Example URLs:

    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/slices
    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/slices/               0x40
### TenantTrafficRuleHandler
Tenat traffic rule queue handler.
#### URLs
    /api/v1/tenants/([a-zA-Z0-9-]*)/trs/?
    /api/v1/tenants/([a-zA-Z0-9-]*)/trs/([a-zA-Z0-9_=,]*)/?
#### GET
List traffic rules .

Args:
    tenant_id: network name of a tenant
    match: the openflow match rule (e.g. dl_vlan=100;tp_dst=80)

Example URLs:

    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/trs
    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/trs/               dl_vlan=100;tp_dst=80
### TenantUEHandler
TenantUE handler. Used to view and manipulate UEs in tenants.
#### URLs
    /api/v1/tenants/([a-zA-Z0-9-]*)/ues/?
    /api/v1/tenants/([a-zA-Z0-9-]*)/ues/([a-zA-Z0-9-]*)/?
#### GET
Get all USe in a Pool or just the specified one.

Args:
    pool_id: the network name
    ue_id: the ue address

Example URLs:
    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/ues
    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/ues/12345
### TenantVBSHandler
TenantVBS Handler.
#### URLs
    /api/v1/tenants/([a-zA-Z0-9-]*)/vbses/?
    /api/v1/tenants/([a-zA-Z0-9-]*)/vbses/([a-zA-Z0-9:]*)/?
#### GET
List PNFDevs.

Args:

    [0]: the tenant id
    [1]: the pnfdev address

Example URLs:

    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/
      <wtps|cpps|vbses>
    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/
      <wtps|cpps|vbses>/11:22:33:44:55:66
### TenantWTPHandler
TenantWTPHandler Handler.
#### URLs
    /api/v1/tenants/([a-zA-Z0-9-]*)/wtps/?
    /api/v1/tenants/([a-zA-Z0-9-]*)/wtps/([a-zA-Z0-9:]*)/?
#### GET
List PNFDevs.

Args:

    [0]: the tenant id
    [1]: the pnfdev address

Example URLs:

    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/
      <wtps|cpps|vbses>
    GET /api/v1/tenants/52313ecb-9d00-4b7d-b873-b55d3d9ada26/
      <wtps|cpps|vbses>/11:22:33:44:55:66
### TrafficRuleHandler
TrafficRule handler. Used to view traffic rules.
#### URLs
    /api/v1/trs/?
#### GET
Lists all the traffic rules managed by this controller.

Args:
    None

Example URLs:
    GET /api/v1/trs
### UEHandler
UE handler. Used to view UEs (controller-wide).
#### URLs
    /api/v1/ues/?
    /api/v1/ues/([a-zA-Z0-9-]*)/?
#### GET
Get all UEs or just the specified one.

Args:
    ue_id: the lvap address

Example URLs:
    GET /api/v1/ues
    GET /api/v1/ues/123345
### VBSHandler
VBS Handler.
#### URLs
    /api/v1/vbses/?
    /api/v1/vbses/([a-zA-Z0-9:]*)/?
#### GET
List PNFDevs.

Args:

    [0]: the pnfdev address

Example URLs:

    GET /api/v1/<wtps|cpps|vbses>
    GET /api/v1/<wtps|cpps|vbses>/11:22:33:44:55:66
### WTPHandler
WTP Handler.
#### URLs
    /api/v1/wtps/?
    /api/v1/wtps/([a-zA-Z0-9:]*)/?
#### GET
List PNFDevs.

Args:

    [0]: the pnfdev address

Example URLs:

    GET /api/v1/<wtps|cpps|vbses>
    GET /api/v1/<wtps|cpps|vbses>/11:22:33:44:55:66
