# "Get Data Source List Info" flow

The "Get Data Source List Info" flow is designed to be executed as a child flow and accepts two parameters: the `site URL` and `library/list Id`, as provided by the environment variables.

It returns list information including:

- **list id**: this is the value provided as `List Id `parameter,
- **drive Id**: used by Graph API when referencing libraries, 
- **list name**: the `RootFolder` as developers know it; used in URL; it's generated automatically and cannot be changed by users (can be changed with PowerShell)
- **list title**: this value can be changed by users; I'm never using it in my code to reference lists, but you may want to use it when sending notifications
- absolute Url (the format of absolute URL differs between lists and libraries).

## Solution contents

The solution consits of:

- **Get Data Source List Info** cloud flow, 
- **test** flow, used for evaluating the "Get Data Source List Info" flow
- **3 environment variables**: v_SiteUrl, v_List and v_Library, used in the **test** flow
- **SharePoint Online** connection reference

## Installation

1. To use the workflow, import either **managed or unmanaged solution** available under [Releases](https://github.com/kkazala/PowerAutomate-Get-Data-Source-List-Info/releases). If you want to be able to edit the flow, choose **unmanaged**:

1. During the import process, update the connection references and environment variables.

1. After the solution is imported, configure the **Get Data Source List Info** cloud flow, to be executed as a child flow. Configured  the `run only` permissions, using the flow owner’s embedded connection:
 ![Run Only Users](./img/runonlyuser.png) 
 Click on **Edit** link and change the connection in the **Connections Used** section
 ![Run Only Connection](./img/runOnlyConnection.png)

1. Add a [service principal](https://learn.microsoft.com/en-us/power-automate/service-principal-support) as an additional owner to ensure business continuity.

## Workflow contents

> ⚠️ Important
>
> You are now installing a workflow from an unknown source in the internet. I appreciate your trust and I'm happy if you find this workflow useful.
> The least you can do to ensure you are not installing malicious flow that will leak data from your company, is to review it. Always check if there are any actions that might send information to an external endpoint. 

###  Scope: get list Id, displayName and Name (used in url)

![](./img/scope1.png)

This action is using `GET` method, which means it's reading information from your SharePoint site.

It retrieves list information for the list defined as a parameter, and and retrieves parameters that are returned by the flow.

### Send an HTTP request to SharePoint: get Drive Id

![alt text](./img/scope2.png)

This action is also only reading (`GET` method) data from your SharePoint site. 
It's using [_api/v2.0/drive](https://learn.microsoft.com/en-us/graph/api/drive-list?view=graph-rest-1.0&tabs=http) api to list availabled drives. 

This api doesn't support filtering, so after retrieving all available drives (libraries) the flow filters results to find the requested library:

![filter results](./img/filter.png)

## Additional resources

[Helpful tips for using Child Flows](https://www.microsoft.com/en-us/power-platform/blog/power-automate/helpful-tips-for-using-child-flows/?msockid=3f7882d233bd676f193e961932e66616)

[Support for service principal owned flows](https://learn.microsoft.com/en-us/power-automate/service-principal-support)

[Service principal application users can own and run flows](https://learn.microsoft.com/en-us/power-platform/release-plan/2023wave1/power-automate/enable-flows-that-are-owned-service-principals)
