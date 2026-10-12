# Big Tech exit map (draft v7)

Three charts by domain. In each, my home is on top, the Pi-hole is the choke point for all outbound traffic, and the upstream companies are below.

## Legend

- Double border = hardware, single border = software/service
- Green = self-hosted by me, red = Big Tech (including their hardware on my network, like the Frame TV and Pixel), orange = managed hosting/provider with negligible telemetry (a flavor of self-hosting), grey = retired
- Thick arrow = steady traffic, thin arrow = light or filtered traffic
- Dotted red = blocked, dotted green = replacement in progress, dashed red box = subscription ending (HBO Max, Nov 1)
- Thick green box = fully offline, no network connection at all (Blu-ray player)

## 1. Media and reading

```mermaid
---
title: "Media and reading: TV, streaming, music, RSS"
---
flowchart TB
    subgraph home["MY HOME NETWORK"]
        direction LR

        subgraph h_media["Media"]
            direction TB
            frame[[Samsung Frame TV]]
            plex[Plex + Plexamp<br/>music server]
        end

        subgraph h_reading["Reading"]
            direction TB
            liveboat[LiveBoat RSS<br/>static site]
        end
    end

    subgraph retired["RETIRED / RETIRING: no traffic"]
        feedly[<s>Feedly</s><br/>fully replaced]
        hbo[HBO Max<br/>expires Nov 1]
    end

    subgraph offline["OFF THE NETWORK: no wifi, no ethernet"]
        bluray[[Blu-ray player<br/>+ disc catalog, in progress]]
    end

    pihole[[Pi-hole<br/>DNS filter for all outbound traffic]]
    blocked[(Blocked:<br/>telemetry, ads, trackers)]

    subgraph out["UPSTREAM"]
        direction LR

        subgraph samsung["Samsung"]
            scloud[TV cloud<br/>+ telemetry]
        end

        subgraph netflix["Netflix"]
            nflx[Streaming]
        end

        subgraph plexinc["Plex Inc"]
            plextv[plex.tv<br/>login only]
        end

        subgraph github["GitHub / Microsoft"]
            ghpages[GitHub Pages<br/>hosts LiveBoat]
        end
    end

    feedly == "100% migrated" ==> liveboat
    hbo -. "replacing with" .-> bluray

    h_media ==> pihole
    h_reading --> pihole
    pihole -.-x blocked
    pihole --> samsung
    pihole ==> netflix
    pihole --> plexinc
    pihole --> github

    classDef big fill:#fdd,stroke:#c33,color:#400
    classDef self fill:#dfd,stroke:#3a3,color:#040
    classDef gate fill:#cdf,stroke:#36c,stroke-width:3px,color:#014
    classDef hosted fill:#ffe6b8,stroke:#d98200,color:#5a3300
    classDef gone fill:#eee,stroke:#999,stroke-dasharray: 2 4,color:#777
    classDef note fill:#ffd,stroke:#cc3,stroke-dasharray: 4 3,color:#440
    class scloud,nflx,hbo,plextv big
    class ghpages hosted
    class frame big
    class plex,liveboat,bluray self
    class pihole gate
    class feedly gone
    class blocked note
    style home fill:#f4fff4,stroke:#3a3,stroke-width:2px
    style out fill:#fffafa,stroke:#c33,stroke-width:2px
    style h_media fill:#e8f8e8,stroke:#3a3,stroke-dasharray: 5 3
    style h_reading fill:#e8f8e8,stroke:#3a3,stroke-dasharray: 5 3
    style retired fill:#f6f6f6,stroke:#999,stroke-dasharray: 2 4
    style offline fill:#e6fff0,stroke:#0a8a3a,stroke-width:4px
    style samsung fill:#fff0f0,stroke:#c33
    style netflix fill:#fff0f0,stroke:#c33
    style hbo stroke-dasharray: 5 3
    style plexinc fill:#fff0f0,stroke:#c33
    style github fill:#fff3dc,stroke:#d98200
```

## 2. Smart home and files

```mermaid
---
title: "Smart home and files: Home Assistant, IoT, sync, backups"
---
flowchart TB
    subgraph home["MY HOME NETWORK"]
        direction LR

        subgraph h_smart["Smart home"]
            direction TB
            ha[Home Assistant<br/>local-first]
            iot_l[[IoT devices: ~30%<br/>local only]]
            iot_c[[IoT devices: ~70%<br/>need internet]]
            ha --> iot_l
            ha --> iot_c
        end

        subgraph h_files["Files"]
            direction TB
            resilio[Resilio Sync<br/>3 nodes]
        end
    end

    pihole[[Pi-hole<br/>DNS filter for all outbound traffic]]

    subgraph out["UPSTREAM"]
        direction LR

        subgraph vendors["IoT vendor clouds"]
            vcloud[Device clouds]
        end

        subgraph nabu["Nabu Casa"]
            nabucloud[Cloud<br/>HA remote access + backups]
        end

        subgraph dropbox["Dropbox"]
            dbx[HA backups only]
        end
    end

    h_smart ==> pihole
    h_files --> pihole
    pihole ==> vendors
    pihole --> nabu
    pihole --> dropbox

    dbx -. "all other files replaced" .-> resilio

    classDef big fill:#fdd,stroke:#c33,color:#400
    classDef self fill:#dfd,stroke:#3a3,color:#040
    classDef gate fill:#cdf,stroke:#36c,stroke-width:3px,color:#014
    classDef hosted fill:#ffe6b8,stroke:#d98200,color:#5a3300
    class vcloud big
    class nabucloud,dbx hosted
    class ha,iot_l,iot_c,resilio self
    class pihole gate
    style home fill:#f4fff4,stroke:#3a3,stroke-width:2px
    style out fill:#fffafa,stroke:#c33,stroke-width:2px
    style h_smart fill:#e8f8e8,stroke:#3a3,stroke-dasharray: 5 3
    style h_files fill:#e8f8e8,stroke:#3a3,stroke-dasharray: 5 3
    style vendors fill:#fff0f0,stroke:#c33
    style nabu fill:#fff3dc,stroke:#d98200
    style dropbox fill:#fff3dc,stroke:#d98200
```

## 3. Personal admin and mobile

```mermaid
---
title: "Personal admin and mobile: email, calendar, phone, carrier"
---
flowchart TB
    subgraph home["MY HOME NETWORK"]
        direction LR

        subgraph h_mobile["Mobile"]
            direction TB
            pixel[[Pixel phone<br/>on wifi]]
        end
    end

    pihole[[Pi-hole<br/>DNS filter for all outbound traffic]]

    subgraph out["UPSTREAM"]
        direction LR

        subgraph google["Google"]
            direction TB

            subgraph g_admin["Personal admin"]
                gmail[Gmail]
                gcal[Calendar]
            end

            subgraph g_mobile["Mobile"]
                gapps[Android / Play services]
                fi[Google Fi<br/>cellular]
            end
        end

        subgraph proton["Proton: personal admin"]
            direction TB
            pmail[Proton Mail]
            pcal[Proton Calendar]
        end
    end

    h_mobile ==> pihole
    pihole ==> google
    pihole --> proton

    pixel -. "on cellular: bypasses Pi-hole" .-> fi
    gmail -. "mid-migration" .-> pmail
    gcal -. "mid-migration" .-> pcal

    classDef big fill:#fdd,stroke:#c33,color:#400
    classDef self fill:#dfd,stroke:#3a3,color:#040
    classDef gate fill:#cdf,stroke:#36c,stroke-width:3px,color:#014
    classDef hosted fill:#ffe6b8,stroke:#d98200,color:#5a3300
    class gmail,gcal,gapps,fi big
    class pmail,pcal hosted
    class pixel big
    class pihole gate
    style home fill:#f4fff4,stroke:#3a3,stroke-width:2px
    style out fill:#fffafa,stroke:#c33,stroke-width:2px
    style h_mobile fill:#e8f8e8,stroke:#3a3,stroke-dasharray: 5 3
    style google fill:#fff0f0,stroke:#c33
    style g_admin fill:#ffe0e0,stroke:#c33,stroke-dasharray: 5 3
    style g_mobile fill:#ffe0e0,stroke:#c33,stroke-dasharray: 5 3
    style proton fill:#fff3dc,stroke:#d98200
```

## Notes

- Assumption: Google Fi on cellular skips Pi-hole. Delete that dotted edge if the phone always tunnels home.
- Orange = GitHub Pages, Dropbox (HA backups only), Nabu Casa, Proton.
- Next inputs: search/browser, password manager, maps, photos, other SaaS.
