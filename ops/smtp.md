# Bil yu yon SMTP mail sɛn sava

## di fɔs pat pan di wɔd dɛn

SMTP kin bay savis dɛn dairekt frɔm klawd vendor dɛn, lɛk:

* [Amazɔn SES SMTP](https://docs.aws.amazon.com/ses/latest/dg/send-email-smtp.html)
* [Ali klawd imel push](https://www.alibabacloud.com/help/directmail/latest/three-mail-sending-methods)

Yu kin bil yu yon mail sava bak - fɔ sɛn we nɔ gɛt limit, ɔl di kɔst nɔ bɔku.

Dɔŋ ya, wi de sho stɛp bay stɛp aw fɔ bil wi yon mɛyl sava.

## Di sɛlɛkshɔn fɔ di sava

Di SMTP sava we de ɔs insɛf nid fɔ gɛt pɔblik IP wit pɔt 25, 456, ɛn 587 we opin.

Di pɔblik klawd dɛn we dɛn kin yuz bɔku tɛm dɔn blok dɛn pɔt dɛn ya bay difɔlt, ɛn i kin pɔsibul fɔ opin dɛn bay we yu gi wok ɔda, bɔt i rili trɔbul afta ɔl.

A rikɔmɛnd fɔ bay frɔm wan ɔs we gɛt dɛn pɔt ya opin ɛn sɔpɔt fɔ sɛt rivas domɛyn nem dɛn.

Na ya, a kin rikɔmɛnd [Contabo](https://contabo.com) .

Contabo na wan hosting provider we de na Munich, Germany, we dɛn bin bil insay 2003 wit prayz we rili kɔmpitishɔn.

If yu pik Yuro as di mɔni fɔ bay, di prayz go shɔt (wan sava wit 8GB mɛmori ɛn 4 CPU dɛn de kɔst lɛk 529 yuan fɔ wan ia, ɛn di fɔs instɔleshɔn fi na fri fɔ wan ia).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/UoAQkwY.webp)

We yu de mek ɔda, rimak `prefer AMD` , ɛn di sava wit AMD CPU go gɛt bɛtɛ pefɔmɛns.

Insay di wan we de kam, a go tek Contabo in VPS as ɛgzampul fɔ sho aw fɔ bil yu yon mail sava.

## Ubuntu sistem kɔnfigyushɔn

Di ɔpreshɔn sistem we de ya na Ubuntu 22.04

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/smpIu1F.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/m7Mwjwr.webp)

If di sava na ssh de sho `Welcome to TinyCore 13!` (as yu si na di pikchɔ we de dɔŋ ya), i min se dɛn nɔ dɔn instɔl di sistɛm yet. Duya diskonɛkt ssh ɛn wet fɔ sɔm minit fɔ log in bak.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/-qKACz9.webp)

`Welcome to Ubuntu 22.04.1 LTS` apia, di initialization dɔn dɔn, ɛn yu kin kɔntinyu wit di step dɛn we de dɔŋ ya.

### [Optional] Initialize di divɛlɔpmɛnt ɛnvayrɔmɛnt

Dis step na opshɔnal tin.

Fɔ mek i izi, a put di instɔleshɔn ɛn sistɛm kɔnfigyushɔn fɔ ubuntu softwe na [github.com/wactax/ops.os/tree/main/ubuntu](https://github.com/wactax/ops.os/tree/main/ubuntu) .

Rɔn di kɔmand we de dɔŋ fɔ instɔl wit wan klik.

```
bash <(curl -s https://raw.githubusercontent.com/wactax/ops.os/main/ubuntu/boot.sh)
```

Di wan dɛn we de yuz Chaynish, duya yuz di kɔmand we de dɔŋ ya insted, ɛn di langwej, di tɛm zon, ɛn ɔda tin dɛn go sɛt ɔtomɛtik wan.

```
CN=1 bash <(curl -s https://ghproxy.com/https://raw.githubusercontent.com/wactax/ops.os/main/ubuntu/boot.sh)
```

### Contabo de ɛnabul IPV6

Enable IPV6 so dat SMTP kin sɛn imel bak wit IPV6 adrɛs.

ɛdit `/etc/sysctl.conf`

Modify ɔ ad di layn dɛn we de dɔŋ ya

```
net.ipv6.conf.all.disable_ipv6 = 0
net.ipv6.conf.default.disable_ipv6 = 0
net.ipv6.conf.lo.disable_ipv6 = 0
```

Fɔ fala [di contabo tutoriɛl: Ad IPv6 kɔnɛktiviti to yu sava](https://contabo.com/blog/adding-ipv6-connectivity-to-your-server/)

Ɛdit `/etc/netplan/01-netcfg.yaml` , ad sɔm layn dɛn lɛk aw dɛn sho na di figa dɔŋ ya (Contabo VPS difɔlt kɔnfigyushɔn fayl dɔn ɔlrɛdi gɛt dɛn layn ya, jɔs nɔ kɔmɛnt dɛn).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/5MEi41I.webp)

Dɔn `netplan apply` fɔ mek di modifyed kɔnfigyushɔn tek ɛfɛkt.

Afta di kɔnfigyushɔn dɔn saksesful, yu kin yuz `curl 6.ipw.cn` fɔ si di ipv6 adrɛs fɔ yu ɛksternal nɛtwɔk.

## Klon di konfigyushɔn ripɔsitɔri ops

```
git clone https://github.com/wactax/ops.soft.git
```

## Jɛnɛret wan fri SSL sɛtifiket fɔ yu domɛyn nem

Fɔ sɛn mɛyl nid fɔ gɛt SSL sɛtifiket fɔ ɛnkripshɔn ɛn sayn.

Wi de yuz [acme.sh](https://github.com/acmesh-official/acme.sh) fɔ jenarayz sɛtifiket dɛn.

acme.sh na wan opin sos ɔtomatik sɛtifiket sayn tul,

Ɛntay di kɔnfigyushɔn westɛm ops.soft, rɔn `./ssl.sh` , ɛn dɛn go mek wan kɔnf `conf` na **di ɔpa dairektrɔ** .

Fɛn yu DNS prɔvayda frɔm [acme.sh dnsapi](https://github.com/acmesh-official/acme.sh/wiki/dnsapi) , ɛdit `conf/conf.sh` .

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/Qjq1C1i.webp)

Dɔn rɔn `./ssl.sh 123.com` fɔ jenarayz `123.com` ɛn `*.123.com` sɛtifiket fɔ yu domɛyn nem.

Di fɔs rɔn go ɔtomɛtik instɔl [acme.sh](https://github.com/acmesh-official/acme.sh) ɛn ad wan scheduled task fɔ ɔtomɛtik rinuɛl. Yu kin si `crontab -l` , na so wan layn de lɛk dis.

```
52 0 * * * "/mnt/www/.acme.sh"/acme.sh --cron --home "/mnt/www/.acme.sh" > /dev/null
```

Di rod fɔ di sɛtifiket we dɛn dɔn jenarayz na sɔntin lɛk `/mnt/www/.acme.sh/123.com_ecc。`

Sɛtifiket rinual go kɔl `conf/reload/123.com.sh` skript, ɛdit dis skript, yu kin ad kɔmand dɛn lɛk `nginx -s reload` fɔ rifresh di sɛtifiket kesh fɔ di aplikeshɔn dɛn we gɛt fɔ du wit am.

## Bil SMTP sava wit chasquid

[chasquid](https://github.com/albertito/chasquid) na wan opin sos SMTP sava we dɛn rayt insay Go langwej.

As substitute fɔ di ol mail server program dɛm lɛk Postfix ɛn Sendmail, chasquid simpul ɛn izi fɔ yuz, ɛn i izi bak fɔ sɛkɔndari divɛlɔpmɛnt.

Run `./chasquid/init.sh 123.com` go instɔl ɔtomɛtik wit wan klik (riples 123.com wit yu sɛn domɛyn nem).

## Konfigyuret Imel Sayn DKIM

Dɛn kin yuz DKIM fɔ sɛn imel sayn fɔ mek dɛn nɔ trit lɛta dɛn lɛk spam.

Afta di kɔmand dɔn rɔn fayn fayn wan, dɛn go tɛl yu fɔ sɛt di DKIM rɛkɔd (as dɛn sho dɔŋ ya).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/LJWGsmI.webp)

Jɔs ad wan TXT rɛkɔd to yu DNS (as dɛn sho dɔŋ ya).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/0szKWqV.webp)

## View savis stetɔs & lɔg dɛn

 `systemctl status chasquid` Si di savis stetus.

di stet fכ nכmal כpεreshכn na lεk aw dεn sho na di fכs we de dכn

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/CAwyY4E.webp)

 `grep chasquid /var/log/syslog` ɔ `journalctl -xeu chasquid` kin si di mistek lɔg.

## Rivas domɛyn nem kɔnfigyushɔn

Di rivays domɛyn nem na fɔ alaw di IP adrɛs fɔ sɔlv to di kɔrɛspɔndɛns domɛyn nem.

If yu sɛt rivas domɛyn nem, dat kin mek dɛn nɔ no se imel dɛn na spam.

We dɛn gɛt di mɛyl, di sava we de gɛt di mɛyl go du rivas domɛyn nem analisis na di IP adrɛs fɔ di sava we de sɛn fɔ kɔnfɔm if di sava we de sɛn gɛt valid rivas domɛyn nem.

If di sava we de sɛn nɔ gɛt rivas domɛyn nem ɔ if di rivas domɛyn nem nɔ mach wit di IP adrɛs fɔ di sava we de sɛn, di sava we de sɛn di imel kin no di imel as spam ɔ nɔ gri fɔ tek am.

Visit [https://my.contabo.com/rdns](https://my.contabo.com/rdns) ɛn kɔnfigyut lɛk aw dɛn sho dɔŋ ya

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/IIPdBk_.webp)

Afta yu dɔn sɛt di rivas domɛyn nem, mɛmba fɔ kɔnfigyut di fɔwad rizɔlt fɔ di domɛyn nem ipv4 ɛn ipv6 to di sava.

## Ɛdit di ɔs nem fɔ chasquid.conf

Modify `conf/chasquid/chasquid.conf` to di valyu fɔ di rivas domɛyn nem.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/6Fw4SQi.webp)

Dɔn rɔn `systemctl restart chasquid` fɔ ristart di savis.

## Backup conf to git ripɔsitɔri

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/Fier9uv.webp)

Fo egzampl, a bak op di conf folda to mi own github proses lek dis

Krio prayvet westɛm fɔs

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/ZSCT1t1.webp)

Enta di conf dairektrɔ ɛn sɔbmit to di westɛm

```
git init
git add .
git commit -m "init"
git branch -M main
git remote add origin git@github.com:wac-tax-key/conf.git
git push -u origin main
```

## Ad di pɔsin we sɛn am

rɔn

```
chasquid-util user-add i@wac.tax
```

Kin ad sender

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/khHjLof.webp)

### Verifay se di paswɔd dɔn sɛt kɔrɛkt wan

```
chasquid-util authenticate i@wac.tax --password=xxxxxxx
```

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/e92JHXq.webp)

Afta yu ad di yuza, `chasquid/domains/wac.tax/users` go ɔpdet, mɛmba fɔ sɛn am na di westɛm.

## DNS ad SPF rεkɔd

SPF ( Sender Policy Framework ) na wan imel verifyeshɔn teknɔlɔji we dɛn kin yuz fɔ mek dɛn nɔ yuz imel fɔ ful pipul dɛn.

I de chɛk if di pɔsin we sɛn di mɛyl in aydentiti bay we i de chɛk if di pɔsin in IP adrɛs de mach wit di DNS rɛkɔd dɛn fɔ di domɛyn nem we i se i bi, ɛn dis kin mek pipul dɛn we de ful pipul dɛn nɔ ebul fɔ sɛn lay lay imel dɛn.

If yu ad SPF rɛkɔd dɛn, dat kin mek dɛn nɔ no se imel dɛn na spam as i pɔsibul.

If yu domɛyn nem sava nɔ sɔpɔt SPF tayp, jɔs ad TXT tayp rɛkɔd.

Fɔ ɛgzampul, di SPF fɔ `wac.tax` na lɛk dis

`v=spf1 a mx include:_spf.wac.tax include:_spf.google.com ~all`

SPF fɔ `_spf.wac.tax`

`v=spf1 a:smtp.wac.tax ~all`

Notis se a don `include:_spf.google.com` ya, dis na bikohs a go konfigyuret `i@wac.tax` as di sending adres na di Google mailbox leta.

## DNS konfigyushɔn DMARC

DMARC na di abbrevieshɔn fɔ (Domain-based Message Authentication, Ripɔtin & Kɔnfɔmɛns).

I de yuz fɔ kech SPF bauns (sɔntɛm na bikɔs ɔf kɔnfigyushɔn mistek, ɔ ɔda pɔsin de mek lɛk se na yu fɔ sɛn spam).

Add TXT rekod `_dmarc` , .

Di tin dɛn we de insay de na lɛk dis

```
v=DMARC1; p=quarantine; fo=1; ruf=mailto:ruf@wac.tax; rua=mailto:rua@wac.tax
```

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/k44P7O3.webp)

Di minin fɔ ɛni paramita na lɛk dis

### p (Polisi) .

I de sho aw fɔ handle imel dɛn we nɔ ebul fɔ SPF (Sender Policy Framework) ɔ DKIM (DomainKeys Identified Mail) verifyeshɔn. Di p paramita kin sɛt to wan pan tri valyu dɛn:

* nɔ de: Dɛn nɔ tek ɛni akshɔn, na di verifyeshɔn rizɔlt nɔmɔ dɛn de fid bak to di pɔsin we sɛn am tru di imel ripɔt mɛkanism.
* Kwarantin: Put di mɛyl we nɔ pas di verifyeshɔn na di spam fɔlda, bɔt nɔ go rijɛkt di mɛyl dairekt.
* rijɛkt: Rijɛkt imel dɛn dairekt wan we nɔ ebul fɔ chɛk.

### fo (Di Opshɔn dɛn we yu nɔ go ebul fɔ du) .

Spɛsifik di amount ɔf infɔmeshɔn we di ripɔt mɛkanism de gi bak. Yu kin sɛt am to wan pan dɛn valyu ya:

* 0: Ripɔt validɛshɔn rizɔlt fɔ ɔl di mɛsej dɛn
* 1: Na di mɛsej dɛn nɔmɔ we dɛn nɔ ebul fɔ chɛk
* d: Na fɔ ripɔt di domɛyn nem verifyeshɔn fɔlt dɛn nɔmɔ
* s: na di ripɔt nɔmɔ we SPF verifyeshɔn nɔ wok
* l: Na onli ripɔt DKIM verifyeshɔn fayl dɛn

### rua & ruf we dɛn kɔl ruf

* rua (Ripɔt URI fɔ Aggregate ripɔt): Imel adrɛs fɔ gɛt aggregɛt ripɔt
* ruf (Ripɔt URI fɔ Fɔrɛns ripɔt): imel adrɛs fɔ gɛt ditayl ripɔt

## Ad MX rεkɔd fɔ fɔwad imel to Google Mail

Bikɔs a nɔ bin ebul fɔ fɛn fri kɔpɔt mɛylbɔks we de sɔpɔt yunivasal adrɛs (Catch-All, kin gɛt ɛni imel we dɛn sɛn to dis domɛyn nem, we nɔ gɛt lɔ pan prɛfiks), a yuz chasquid fɔ fɔwad ɔl di imel dɛn to mi Gmail mɛylbɔks.

**If yu gɛt yu yon pe biznɛs mɛylbɔks, duya nɔ chenj di MX ɛn skip dis step.**

Ɛdit `conf/chasquid/domains/wac.tax/aliases` , sɛt fɔwad mɛylbɔks

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/OBDl2gw.webp)

`*` de sho ɔl di imel dɛn, `i` na di imel adrɛs prɛfiks fɔ di pɔsin we de sɛn we dɛn mek ɔp. Fɔ sɛn mɛyl, ɛnibɔdi we de yuz am nid fɔ ad wan layn.

Dɔn ad di MX rɛkɔd (a de pɔynt dairekt to di adrɛs fɔ di rivas domɛyn nem ya, lɛk aw dɛn sho na di fɔs layn na di figa dɔŋ ya).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/7__KrU8.webp)

Afta di kɔnfigyushɔn dɔn, yu kin yuz ɔda imel adrɛs fɔ sɛn imel to `i@wac.tax` ɛn `any123@wac.tax` fɔ si if yu kin gɛt imel na Gmail.

If nɔto so, chɛk di chasquid lɔg ( `grep chasquid /var/log/syslog` ).

## Send wan imel to i@wac.tax wit Google Mail

Afta Google Mail get di mail, a naturally op se a go riply wit `i@wac.tax` insted of i.wac.tax@gmail.com.

Visit [https://mail.google.com/mail/u/1/#settings/accounts](https://mail.google.com/mail/u/1/#settings/accounts) ɛn klik "Add another email address".

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/PAvyE3C.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/_OgLsPT.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/XIUf6Dc.webp)

Dɔn, ɛnta di verifyeshɔn kɔd we di imel we dɛn bin fɔwad to dɔn gɛt.

Fɔ dɔn, yu kin sɛt am as di difɔlt sɛnda adrɛs (wit di opshɔn fɔ ansa wit di sem adrɛs).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/a95dO60.webp)

Dis we ya, wi dɔn dɔn fɔ mek di SMTP mɛyl sava ɛn di sem tɛm yuz Google Mail fɔ sɛn ɛn gɛt imel.

## Send wan tɛst imel fɔ chɛk if di kɔnfigyushɔn dɔn wok fayn

Ɛntay `ops/chasquid`

Rɔn `direnv allow` fɔ instɔl dipɛnsin dɛn (direnv dɔn instɔl insay di fɔs wan-ki initializayshɔn prɔses ɛn dɛn dɔn ad huk to di shɔɛl)

dɔn rɔn

```
user=i@wac.tax pass=xxxx to=iuser.link@gmail.com ./sendmail.coffee
```

Di minin fɔ di paramita dɛn na lɛk dis

* yuz: SMTP yuz nem
* pas: SMTP paswɔd
* to: di pɔsin we de gɛt am

Yu kin sɛn wan tɛst imel.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/ae1iWyM.webp)

I fayn fɔ yuz Gmail fɔ gɛt tɛst imel fɔ chɛk if di kɔnfigyushɔn dɛn dɔn wok fayn.

### TLS standad enkripshɔn

As yu si na di pikchɔ we de dɔŋ ya, dis smɔl lɔk de, we min se dɛn dɔn ɛnabul di SSL sɛtifiket fayn fayn wan.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/SrdbAwh.webp)

Dɔn klik "Show Original Email".

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/qQQsdxg.webp)

### DKIM we dɛn kɔl DKIM

As yu si na di pikchɔ we de dɔŋ ya, di Gmail ɔrijinal mɛyl pej de sho DKIM, we min se di DKIM kɔnfigyushɔn dɔn wok fayn.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/an6aXK6.webp)

Chek di Risiv insay di hεda fɔ di ɔrijinal imel, ɛn yu go si se di adrɛs we sɛn am na IPV6, we min se IPV6 sɛf dɔn kɔnfigyut fayn fayn wan.
