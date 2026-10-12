# Big Tech exit map

Four charts by domain. In each, my home is on top, a DNS filter sits in front of outbound traffic, and the upstream companies are below.

## Legend


```mermaid
---
config:
  theme: base
  themeVariables:
    textColor: '#222222'
    edgeLabelBackground: '#ffffff'
    lineColor: '#888888'
---
flowchart TB
    subgraph colors["Who runs it"]
        direction LR
        c1[Self-hosted<br/>by me]
        c2[Managed provider,<br/>low telemetry]
        c3[Big Tech, incl.<br/>their hardware]
        c4[Retired]
        c1 ~~~ c2 ~~~ c3 ~~~ c4
    end

    subgraph shapes["Shapes and borders"]
        direction LR
        s1[Software]
        s2[[Hardware]]
        s3[Subscription ending]
        s4[Fully offline]
        s1 ~~~ s2 ~~~ s3 ~~~ s4
    end

    subgraph edges["Arrows"]
        direction LR
        a1[ ] ==>|steady| a2[ ]
        a3[ ] -->|light| a4[ ]
        a5[ ] -.->|replacing| a6[ ]
        a7[ ] -.-x|blocked| a8[ ]
    end

    classDef self fill:#dfd,stroke:#3a3,color:#040
    classDef hosted fill:#ffe6b8,stroke:#d98200,color:#5a3300
    classDef big fill:#fdd,stroke:#c33,color:#400
    classDef gone fill:#eee,stroke:#999,stroke-dasharray: 2 4,color:#777
    classDef blank fill:none,stroke:none,color:#fff
    class c1,s1,s2 self
    class c2 hosted
    class c3 big
    class c4 gone
    class s3 big
    class s4 self
    class a1,a2,a3,a4,a5,a6,a7,a8 blank
    style s3 stroke-dasharray: 5 3
    style s4 stroke-width:4px,stroke:#0a8a3a
    style colors fill:#fff,stroke:#999,color:#222
    style shapes fill:#fff,stroke:#999,color:#222
    style edges fill:#fff,stroke:#999,color:#222
```

## 1. Media and reading

```mermaid
---
config:
  theme: base
  themeVariables:
    textColor: '#222222'
    titleColor: '#222222'
    edgeLabelBackground: '#ffffff'
    lineColor: '#888888'
---
flowchart TB
    subgraph home["MY HOME NETWORK"]
        direction LR

        subgraph h_media["Media"]
            direction TB
            frame[[Smart TV]]
            plex[Plex + Plexamp<br/>music server]
        end

        subgraph h_reading["Reading"]
            direction TB
            liveboat[LiveBoat RSS<br/>static site]
        end
    end

    subgraph retired["RETIRED / RETIRING: no traffic"]
        feedly[<s>Feedly</s><br/>fully replaced]
        hbo[HBO Max<br/>ending]
    end

    subgraph offline["OFF THE NETWORK: no network"]
        bluray[[Blu-ray player<br/>+ disc catalog]]
    end

    pihole[[Pi-hole<br/>DNS filter]]
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
    style home fill:#f4fff4,stroke:#3a3,stroke-width:2px,color:#222
    style out fill:#fffafa,stroke:#c33,stroke-width:2px,color:#222
    style h_media fill:#e8f8e8,stroke:#3a3,stroke-dasharray: 5 3,color:#222
    style h_reading fill:#e8f8e8,stroke:#3a3,stroke-dasharray: 5 3,color:#222
    style retired fill:#f6f6f6,stroke:#999,stroke-dasharray: 2 4,color:#222
    style offline fill:#e6fff0,stroke:#0a8a3a,stroke-width:4px,color:#222
    style samsung fill:#fff0f0,stroke:#c33,color:#222
    style netflix fill:#fff0f0,stroke:#c33,color:#222
    style hbo stroke-dasharray: 5 3
    style plexinc fill:#fff0f0,stroke:#c33,color:#222
    style github fill:#fff3dc,stroke:#d98200,color:#222
```

## 2. Smart home and files

```mermaid
---
config:
  theme: base
  themeVariables:
    textColor: '#222222'
    titleColor: '#222222'
    edgeLabelBackground: '#ffffff'
    lineColor: '#888888'
---
flowchart TB
    subgraph home["MY HOME NETWORK"]
        direction LR

        subgraph h_smart["Smart home"]
            direction TB
            ha[Home Assistant<br/>local-first]
            iot_l[[IoT devices<br/>local only]]
            iot_c[[IoT devices<br/>need internet]]
            ha --> iot_l
            ha --> iot_c
        end

        subgraph h_files["Files"]
            direction TB
            resilio[Resilio Sync<br/>multi-node]
        end
    end

    pihole[[Pi-hole<br/>DNS filter]]

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
    style home fill:#f4fff4,stroke:#3a3,stroke-width:2px,color:#222
    style out fill:#fffafa,stroke:#c33,stroke-width:2px,color:#222
    style h_smart fill:#e8f8e8,stroke:#3a3,stroke-dasharray: 5 3,color:#222
    style h_files fill:#e8f8e8,stroke:#3a3,stroke-dasharray: 5 3,color:#222
    style vendors fill:#fff0f0,stroke:#c33,color:#222
    style nabu fill:#fff3dc,stroke:#d98200,color:#222
    style dropbox fill:#fff3dc,stroke:#d98200,color:#222
```

## 3. Personal admin and mobile

```mermaid
---
config:
  theme: base
  themeVariables:
    textColor: '#222222'
    titleColor: '#222222'
    edgeLabelBackground: '#ffffff'
    lineColor: '#888888'
---
flowchart TB
    subgraph home["MY HOME NETWORK"]
        direction LR

        subgraph h_mobile["Mobile"]
            direction TB
            pixel[[Smartphone<br/>on wifi]]
            gallery[Gallery app<br/>local photos]
            brave_m[Brave mobile]
        end

        subgraph h_desktop["Desktop"]
            direction TB
            chrome[Chrome desktop]
        end
    end

    pihole[[Pi-hole<br/>DNS filter]]

    subgraph out["UPSTREAM"]
        direction LR

        subgraph google["Google"]
            direction TB

            subgraph g_admin["Personal admin"]
                gmail[Gmail]
                gcal[Calendar]
            end

            subgraph g_photos["Photos"]
                gphotos[Google Photos]
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

        subgraph bitwarden["Bitwarden: open source"]
            bw[Password manager]
        end

        subgraph brave["Brave"]
            bravecloud[Browser services]
        end
    end

    h_mobile ==> pihole
    h_desktop ==> pihole
    pihole ==> google
    pihole --> proton
    pihole --> bitwarden
    pihole --> brave

    pixel -. "on cellular: bypasses filter" .-> fi
    gmail -. "mid-migration" .-> pmail
    gcal -. "mid-migration" .-> pcal
    gphotos -. "mid-migration" .-> gallery

    classDef big fill:#fdd,stroke:#c33,color:#400
    classDef self fill:#dfd,stroke:#3a3,color:#040
    classDef gate fill:#cdf,stroke:#36c,stroke-width:3px,color:#014
    classDef hosted fill:#ffe6b8,stroke:#d98200,color:#5a3300
    class gmail,gcal,gapps,fi,gphotos,chrome big
    class pmail,pcal,bw,bravecloud,brave_m hosted
    class gallery self
    class pixel big
    class pihole gate
    style home fill:#f4fff4,stroke:#3a3,stroke-width:2px,color:#222
    style out fill:#fffafa,stroke:#c33,stroke-width:2px,color:#222
    style h_mobile fill:#e8f8e8,stroke:#3a3,stroke-dasharray: 5 3,color:#222
    style h_desktop fill:#e8f8e8,stroke:#3a3,stroke-dasharray: 5 3,color:#222
    style google fill:#fff0f0,stroke:#c33,color:#222
    style g_admin fill:#ffe0e0,stroke:#c33,stroke-dasharray: 5 3,color:#222
    style g_mobile fill:#ffe0e0,stroke:#c33,stroke-dasharray: 5 3,color:#222
    style proton fill:#fff3dc,stroke:#d98200,color:#222
    style bitwarden fill:#fff3dc,stroke:#d98200,color:#222
    style brave fill:#fff3dc,stroke:#d98200,color:#222
    style g_photos fill:#ffe0e0,stroke:#c33,stroke-dasharray: 5 3,color:#222
```

## 4. AI and dev tools

```mermaid
---
config:
  theme: base
  themeVariables:
    textColor: '#222222'
    titleColor: '#222222'
    edgeLabelBackground: '#ffffff'
    lineColor: '#888888'
---
flowchart TB
    subgraph home["MY HOME NETWORK"]
        direction LR

        subgraph h_ai["AI"]
            direction TB
            localllm[Local LLM<br/>planned]
        end

        subgraph h_dev["Dev tools"]
            direction TB
            editor[Editor + CLI<br/>coding agents]
        end
    end

    pihole[[Pi-hole<br/>DNS filter]]

    subgraph out["UPSTREAM"]
        direction LR

        subgraph copilot["GitHub / Microsoft"]
            gh[GitHub Copilot<br/>heavy use]
        end

        subgraph claude["Anthropic"]
            cl[Claude models<br/>via Copilot]
        end

        subgraph proton["Proton"]
            lumo[Lumo AI<br/>free tier, ad-hoc]
        end
    end

    h_dev ==> pihole
    pihole ==> copilot
    pihole --> proton
    gh --> cl

    gh -. "replacing with, planned" .-> localllm

    classDef big fill:#fdd,stroke:#c33,color:#400
    classDef self fill:#dfd,stroke:#3a3,color:#040
    classDef gate fill:#cdf,stroke:#36c,stroke-width:3px,color:#014
    classDef hosted fill:#ffe6b8,stroke:#d98200,color:#5a3300
    class gh,cl big
    class lumo hosted
    class editor,localllm self
    class pihole gate
    style localllm stroke-dasharray: 5 3
    style home fill:#f4fff4,stroke:#3a3,stroke-width:2px,color:#222
    style out fill:#fffafa,stroke:#c33,stroke-width:2px,color:#222
    style h_ai fill:#e8f8e8,stroke:#3a3,stroke-dasharray: 5 3,color:#222
    style h_dev fill:#e8f8e8,stroke:#3a3,stroke-dasharray: 5 3,color:#222
    style copilot fill:#fff0f0,stroke:#c33,color:#222
    style claude fill:#fff0f0,stroke:#c33,color:#222
    style proton fill:#fff3dc,stroke:#d98200,color:#222
```
## Notes

- Orange = GitHub Pages, Dropbox (backups only), Nabu Casa, Proton (incl. Lumo), Bitwarden, Brave.
- Next: search, maps, other SaaS.
