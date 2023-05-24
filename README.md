## ARCHITECTURE
**N.B.**: The word terminal shall be used in the following diagrams to represent client devices; phones or computers.

Where:
- **Blue**: Internal Connectivity
- **Magenta**: Un-proxied Connection
- **Orange**: Secure, proxied connection

All cluster pods' images are created to be atomically ephemeral and are running with de-escalated privilidges to provide the latest stable and secure experience. Same holds for OS images deployed via the `boot server` for use by the terminals.
### MODEL A; User Account Mirroring
```mermaid
flowchart LR
%% Variable Declaration %%
classDef blue fill:#2374f7, stroke:#000, stroke-width:2px, colour:#fff
classDef orange fill:#fc822b, stroke:#000, stroke-width:2px, colour:#fff
classDef green fill:#16b552, stroke:#000, stroke-width:2px, colour:#fff
classDef red fill:#ed2633, stroke:#000, stroke-width:2px, colour:#fff
classDef magenta fill:magenta, stroke:#000, stroke-width:2px, colour:#fff
    
    %% k8s | Proxied%%
    subgraph Cloud Cluster
    %% 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 %%
    b([Status Monitor]):::blue --- a[Controller]
    c([VPN]):::blue --- a
    d([Boot Server]):::blue --- a
    e[(Identity Manager)]:::blue --- a
    f([Cloud]):::blue --- a
    g([Office]):::blue --- a
    h([SSL]):::blue --- a
    i([Firewall]):::blue --- a
    j[(File System)]:::blue --- a
    k([Remote Desktop]):::blue --- a
    l([Network Filtering]):::blue --- a
    end

    %% On-Prem %%
    %% 11 %%
    subgraph On-Prem
    m[Router/Gateway/Firewall]
    n[Terminals]:::blue --- m
    end

    %% Remote %%
    subgraph Remote
    o[Remote Terminals]
    end

    %% Mail %%
    %% 12, 13 %%
    subgraph Mail
    p[Mail] -.- a -.-> e
    end

    %% CRM %%
    subgraph CRM
    %% 14, 15%%
    q[CRM] -.- a -.-> e
    end

    %% VOIP %%
    subgraph VOIP
    %% 16, 17 %%
    r[VOIP]-.- a -.-> e
    end

    %% Proxy %%
    %% 18, 19, 20, 21, 22 %%
    subgraph Proxy
    m -.- s[Proxy]:::orange
    o -.- s 
    s -.- a -.- c -.-> e

    end

    %% Link Colours %%
    linkStyle 0,1,2,3,4,5,6,7,8,9,10,11 stroke:blue
    linkStyle 12,13,14,15,16,17 stroke:magenta
    linkStyle 18,19,20,21,22 stroke:orange

```
### MODEL B; Active SSO - No Duplication | RECOMMENDED
```mermaid
flowchart LR
%% Variable Declaration %%
classDef blue fill:#2374f7, stroke:#000, stroke-width:2px, colour:#fff
classDef orange fill:#fc822b, stroke:#000, stroke-width:2px, colour:#fff
classDef green fill:#16b552, stroke:#000, stroke-width:2px, colour:#fff
classDef red fill:#ed2633, stroke:#000, stroke-width:2px, colour:#fff
classDef magenta fill:magenta, stroke:#000, stroke-width:2px, colour:#fff
    
    %% k8s | Proxied%%
    subgraph Cloud Cluster
    %% 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 %%
    b([Status Monitor]):::blue --- a[Controller]
    c([VPN]):::blue --- a
    d([Boot Server]):::blue --- a
    e[(Identity Manager)]:::blue --- a
    f([Cloud]):::blue --- a
    g([Office]):::blue --- a
    h([SSL]):::blue --- a
    i([Firewall]):::blue --- a
    j[(File System)]:::blue --- a
    k([Remote Desktop]):::blue --- a
    l([Network Filtering]):::blue --- a
    end

    %% On-Prem %%
    %% 11 %%
    subgraph On-Prem
    m[Router/Gateway/Firewall]
    n[Terminals]:::blue --- m
    end

    %% Remote %%
    subgraph Remote
    o[Remote Terminals]
    end

    %% Mail %%
    %% 12, 13 %%
    subgraph Mail
    p[Mail] -.- a -.-> e
    end

    %% CRM %%
    subgraph CRM
    %% 14, 15%%
    q[CRM] -.- a -.-> e
    end

    %% VOIP %%
    subgraph VOIP
    %% 16, 17 %%
    r[VOIP]-.- a -.-> e
    end

    %% Proxy %%
    %% 18, 19, 20, 21, 22 %%
    subgraph Proxy
    m -.- s[Proxy]:::orange
    o -.- s 
    s -.- a -.- c -.-> e

    end

    %% Link Colours %%
    linkStyle 0,1,2,3,4,5,6,7,8,9,10,11 stroke:blue
    linkStyle 12,13,14,15,16,17,18,19,20,21,22 stroke:orange
```
### MODEL C; SEPARATE BACKEND
```mermaid
flowchart LR
%% Variable Declaration %%
classDef blue fill:#2374f7, stroke:#000, stroke-width:2px, colour:#fff
classDef orange fill:#fc822b, stroke:#000, stroke-width:2px, colour:#fff
classDef green fill:#16b552, stroke:#000, stroke-width:2px, colour:#fff
classDef red fill:#ed2633, stroke:#000, stroke-width:2px, colour:#fff
classDef magenta fill:magenta, stroke:#000, stroke-width:2px, colour:#fff
    
    %% k8s | Proxied%%
    subgraph Cloud Cluster
    %% 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 %%
    b([Status Monitor]):::blue --- a[Controller]
    c([VPN]):::blue --- a
    d([Boot Server]):::blue --- a
    e[(Identity Manager)]:::blue --- a
    f([Cloud]):::blue --- a
    g([Office]):::blue --- a
    h([SSL]):::blue --- a
    i([Firewall]):::blue --- a
    j[(File System)]:::blue --- a
    k([Remote Desktop]):::blue --- a
    l([Network Filtering]):::blue --- a
    end

    %% On-Prem %%
    %% 11 %%
    subgraph On-Prem
    m[Router/Gateway/Firewall]
    n[Terminals]:::blue --- m
    end

    %% Remote %%
    subgraph Remote
    o[Remote Terminals]
    end

    %% Proxy %%
    %% 12, 13, 14, 15, 16 %%
    subgraph Proxy
    m -.- s[Proxy]:::orange
    o -.- s 
    s -.- a -.- c -.-> e

    end

    %% Link Colours %%
    linkStyle 0,1,2,3,4,5,6,7,8,9,10,11 stroke:blue
    linkStyle 12,13,14,15,16 stroke:orange
```
