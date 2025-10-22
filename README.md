# VR-car remote demó tudnivalók

## Q épületbeli helyszín (ahol az autó van)


### Q épület teendők nap elején

1. Kiszolgáló PC, webkamera + állvány kivitele a QBF14-ből az aulába
2. VR-car kivitele az aulába (akár ki is lehet vezetni, ha már él a konfig a külső helyszínen)


### Demó elindítása

4. Akkuk csatlakoztatása: ólom, LiPo (+lipo-őr!)
5. Autó bekapcs, Nuc külön bekapcs
6. Kiszolgáló PC (erpc) bekapcs (user: bmeaut, pass: enrob123)
7. Kiszolgáló PC csatlakoztatása az autó wifi hálózatára (*5G_CPE_ROBOT* vagy *VrCar_Main_24*, attól függően, hogy melyik van beüzemelve)
8. Külső webkamera (Logitech Brio) csatlakoztatása a kiszolgáló PC-hez (**mindenképpen USB3 portba legyen dugva!**)
9.  Kiszolgáló PC terminálból SSH a vr-car-ra a **lokális wifin keresztül** (nuc-black vagy nuc-red), majd a demó launch file elindítása:
```bash
# Black car:
bmeaut@erpc:~$ ssh nuc@nuc-black.local   # nuc-black IP: 192.168.100.36
nuc@nuc-black:~$ start-nav-zed-half

# Red car:
bmeaut@erpc:~$ ssh nuc@nuc-red.local   # nuc-red IP: 192.168.100.114
nuc@nuc-red:~$ start-nav-zed-half
```

5. Kiszolgáló PC további terminálokban a következőket kell/lehet elindítani:

```bash
# Terminal 2: (RViz indítása)
bmeaut@erpc:~$ master_black  # vagy master_red
bmeaut@erpc:~$ rviz_black    # vagy rviz_red

# Terminal 3: (Fix webkamerakép stream a VR notebook felé)
bmeaut@erpc:~$ ./videostreaming.sh

# Terminal 4: (VR notebook felől webkamerakép fogadása, csak ha muszáj)
bmeaut@erpc:~$ ./stream_in.sh
```

### Q épület teendők nap végén

1. Mindent elpakolni és visszavinni a QBF14-be (asztal és székek maradnak)





## Remote site (ahol a vezetőállás van)

### Remote site teendők nap elején

1. 5G CPE, monitorok, VR sisak, kormány, notebook összekötése, beüzemelése
2. A VR notebook Ethernet kábellel csatlakozzon az 5G CPE-hez!
3. Demó elindítása, letesztelése


### Demó elindítása

1. Csatlakozás az OpenVPN szerverhez: 
   - Windows tálcán jobb katt az "OpenVPN GUI"-ra
   - Felugró context menüben: vrnote-vrzed3 > Connect
   - Ha sikeres volt, kapnunk kellett egy 10.8.0.12-es IP-t a VPN szervertől 
2. Győződjünk meg róla, hogy már fut a vr-car-on a launch (beszéljünk a Q épületben lévőkkel)
3. VR notebook desktop-on "**FFmpegMemoryMap 1280**" elindítása (autóról érkező kamerakép fogadó segédprogram)
4. VR notebook desktop-on "**ZEDMonoDemo black**" vagy "**ZEDMonoDemo red**" elindítása (Unity program build-elt változata)
5. Ha nem megy valamiért a lefordított Unity program, akkor próbáljuk meg a Unity Hub-on keresztül a Unity fejlesztőkörnyezetből:
    - "ZEDMonoDemo" projekt megnyitása
    - Assets > ConfigFiles > red_car konfigban ellenőrizzük az IP címet (Nuc_ip): 
      - nuc-black esetén: 10.8.0.21
      - nuc-red esetén: 10.8.0.20
    - Indítsuk el a "Play" gombbal
6. Fix webkamerakép fogadása a Q épületből: VR notebook desktop-on "**stream_in.bat**" elindítása
7. RustDesk csatlakozás a Q épületbeli kiszolgáló PC-hez: RustDesk > bmeaut@erpc
8. Rendezzük el az ablakokat a két nagy monitoron:
   - Unity ablak (autó kamerakép) az egyikre (ez legyen a notebook kijelzőn is)
   - Távoli fix kamerakép + RustDesk (RViz) a másikra
9.  Webkamerakép küldése a Q épület felé (opcionális): VR notebook desktop-on "**stream_out.bat**" elindítása


### Remote site teendők nap végén

1. VR notebook-ot el kell pakolni a táskájába és hazavinni annak, aki másnap reggel is a külső helyszínen kezd
2. 5G CPE, VR sisak elpakolása a zárható terembe

## IP címek

**nuc-black**:
  - VPN távolról: `10.8.0.21`
  - Lokális wifin: `192.168.100.36`

**nuc-red**:
  - VPN távolról: `10.8.0.20`
  - Lokális wifin: `192.168.100.114`

**vrnote**:
  - VPN távolról: `10.8.0.12`

**erpc** (ros-pc2):
  - VPN távolról: `10.8.0.13`

**vrzed3** SSH elérés (a VPN szerver ezen fut):
  - BME belső hálózatból: `bmeaut@152.66.189.175`
  - VPN-en keresztül: `bmeaut@10.8.0.1`
