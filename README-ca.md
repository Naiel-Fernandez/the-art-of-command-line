🌍
*[Čeština](README-cs.md) ∙ [Deutsch](README-de.md) ∙ [Ελληνικά](README-el.md) ∙ [English](README.md) ∙ [Español](README-es.md) ∙ [Français](README-fr.md) ∙ [Indonesia](README-id.md) ∙ [Italiano](README-it.md) ∙ [日本語](README-ja.md) ∙ [한국어](README-ko.md) ∙ [polski](README-pl.md) ∙ [Português](README-pt.md) ∙ [Română](README-ro.md) ∙ [Русский](README-ru.md) ∙ [Slovenščina](README-sl.md) ∙ [Українська](README-uk.md) ∙ [简体中文](README-zh.md) ∙ [繁體中文](README-zh-Hant.md)*


# L'Art de la Línia d'Ordres

*Nota: Estic planejant revisar això i buscant un nou coautor per ajudar a ampliar-ho a una guia més exhaustiva. Tot i que és molt popular, podria ser més ampli i una mica més profund. Si us agrada escriure i esteu a punt de ser un expert en aquest material i esteu disposats a considerar ajudar, si us plau deixeu-me una nota a josh (0x40) holloway.com. –jjlevy).(https://github.com/jlevy), ).Holloway).(https://www.holloway.com). Gràcies!*

- [Meta](#meta)
- [Basics](#basics)
- [Everyday use](#everyday-use)
- [Processing files and data](#processing-files-and-data)
- [System debugging](#system-debugging)
- [One-liners](#one-liners)
- [Obscure but useful](#obscure-but-useful)
- [macOS only](#macos-only)
- [Windows only](#windows-only)
- [More resources](#more-resources)
- [Disclaimer](#disclaimer)

![curl -s 'https://raw.githubusercontent.com/jlevy/the-art-of-command-line/master/README.md' | egrep -o '`\w+`' | tr -d '`' | cowsay -W50](cowsay.png)

La fluïdesa en la línia de comandament és una habilitat sovint ignorada o considerada arcana, però millora la vostra flexibilitat i productivitat com a enginyer de maneres òbvies i subtils. Aquesta és una selecció de notes i consells sobre l'ús de la línia d'ordres que hem trobat útil quan treballem en Linux. Algunes puntes són elementals, i algunes són bastant específiques, sofisticades o fosques. Aquesta pàgina no és llarga, però si pots utilitzar i recordar tots els elements aquí, saps molt.

Aquesta obra és el resultat de ).molts autors i traductors).(AUTHORS.md).
Una mica
[originally](http://www.quora.com/What-are-some-lesser-known-but-useful-Unix-commands)
[appeared](http://www.quora.com/What-are-the-most-useful-Swiss-army-knife-one-liners-on-Unix)
on [Quora](http://www.quora.com/What-are-some-time-saving-tips-that-every-Linux-user-should-know),
Però des de llavors s'ha traslladat a GitHub, on la gent amb més talent que l'autor original ha fet nombroses millores.

[**Envieu una pregunta**](https://airtable.com/shrzMhx00YiIVAWJg) si teniu una pregunta relacionada amb la línia d'ordres. [**Si us plau, contribuïu**](/CONTRIBUTING.md) si veieu un error o alguna cosa que podria ser millor!

## Meta

Àmbit:

- Aquesta guia és per a usuaris principiants i experimentats. Els objectius són *amplitud* (tot el que és important), *especificitat* (donar exemples concrets del cas més comú) i *brevetat* (evitar coses que no són essencials o digressions que podeu buscar fàcilment en altres llocs). Cada consell és essencial en alguna situació o estalvia molt temps respecte a alternatives.
- Això està escrit per a Linux, amb l'excepció de les seccions "[només macos](només macos)" i "[només Windows](només per a Windows)". Molts dels altres elements s'apliquen o es poden instal·lar en altres Unices o macOS (o fins i tot Cygwin).
- El focus se centra en Bash interactiu, tot i que molts consells s'apliquen a altres shells i als scripts generals de Bash.
- Inclou ordres Unix "estàndards" i altres que requereixen instal·lacions especials de paquets, sempre que siguin prou importants com per merèixer la seva inclusió.

Notes:

- Per mantenir-ho en una pàgina, el contingut s'inclou implícitament per referència. Sou prou intel·ligent per buscar més detalls en altres llocs un cop conegueu la idea o l'ordre de Google. Utilitzeu `apt`, `yum`, `dnf`, `pacman`, `pip` o `brew` (segons correspongui) per instal·lar programes nous.
- Utilitzeu [Explainshell](http://explainshell.com/) per obtenir un desglossament útil de què fan les ordres, les opcions, les canonades, etc.


## Conceptes bàsics

- Aprendre Bash bàsic. De fet, escriviu `man bash` i, almenys, llegiu-ho tot; és bastant fàcil de seguir i no tant llarg. Les petxines alternatives poden ser agradables, però Bash és potent i sempre està disponible (aprendre *només* zsh, fish, etc., mentre que tempta el vostre propi ordinador portàtil, us restringeix en moltes situacions, com ara utilitzar servidors existents).

- Apreneu bé almenys un editor de text. L'editor `nano` és un dels més senzills per a l'edició bàsica (obrir, editar, desar, cercar). Tanmateix, per a l'usuari avançat en un terminal de text, no hi ha cap substitut per Vim (`vi`), l'editor difícil d'aprendre però venerable, ràpid i amb totes les funcions. Molta gent també utilitza el clàssic Emacs, especialment per a tasques d'edició més grans. (Per descomptat, és poc probable que qualsevol desenvolupador de programari modern que treballi en un projecte extens utilitzi només un editor basat en text i també hauria d'estar familiaritzat amb els IDE i les eines gràfiques modernes.)

- Recerca de documentació:
  - Saber llegir documentació oficial amb `man` (per als curiosos, `man man` enumera els números de secció, per exemple, 1 és ordres "normals", 5 és fitxers/convencions i 8 són per a l'administració). Cerca pàgines de manual amb `apropos`.
  - Sapigueu que algunes ordres no són executables, sinó integrades de Bash, i que podeu obtenir ajuda amb `help` i `help -d`. Podeu esbrinar si una ordre és un executable, un shell integrat o un àlies utilitzant "type command".
  - `curl cheat.sh/command` donarà un breu "cheat sheet" amb exemples habituals de com utilitzar una ordre de shell.

- Obteniu informació sobre la redirecció de la sortida i l'entrada amb `>` i `<` i les canonades amb `|`. Know `>` sobreescriu el fitxer de sortida i `>>` s'afegeix. Més informació sobre stdout i stderr.

- Obteniu informació sobre l'expansió del globus de fitxers amb `*` (i potser `?` i `[`...`]`) i entre cometes i la diferència entre cometes dobles `"` i simples `'`. (Vegeu més informació sobre l'expansió variable baix.)

- Estar familiaritzat amb la gestió de treballs de Bash: `&`, **ctrl-z**, **ctrl-c**, `jobs`, `fg`, `bg`, `kill`, etc.

- Coneix `ssh`, i els fonaments bàsics de l'autenticació sense contrasenya, mitjançant `ssh-agent`, `ssh-add`, etc.

- Gestió bàsica de fitxers: `ls` i `ls -l` (en particular, apreneu què significa cada columna de `ls -l`), `less`, `head`, `tail` i `tail -f` (o encara millor, `menys +F`), `ln` i `ln -s` (coneixeu les diferències i els avantatges dels enllaços durs i suaus), `chown`, `chmod`, `du` (per obtenir un resum ràpid del disc ús: `du -hs *`). Per a la gestió del sistema de fitxers, `df`, `mount`, `fdisk`, `mkfs`, `lsblk`. Apreneu què és un inode (`ls -i` o `df -i`).

- Gestió bàsica de la xarxa: `ip` o `ifconfig`, `dig`, `traceroute`, `route`.

- Aprendre i utilitzar un sistema de gestió de control de versions, com ara `git`.

- Conèixer bé les expressions regulars i les diferents banderes de `grep`/`egrep`. Val la pena conèixer les opcions `-i`, `-o`, `-v`, `-A`, `-B` i `-C`.

- Apreneu a utilitzar `apt-get`, `yum`, `dnf` o `pacman` (segons la distribució) per trobar i instal·lar paquets. I assegureu-vos que teniu `pip` per instal·lar eines de línia d'ordres basades en Python (algunes a continuació són les més fàcils d'instal·lar mitjançant `pip`).


## Ús diari

- A Bash, utilitzeu **Tab** per completar arguments o llistar totes les ordres disponibles i **ctrl-r** per cercar a través de l'historial d'ordres (després de prémer, escriviu per cercar, premeu **ctrl-r** repetidament per fer un cicle). a través de més coincidències, premeu **Enter** per executar l'ordre trobat o premeu la fletxa dreta per posar el resultat a la línia actual per permetre l'edició).
- A Bash, utilitzeu **ctrl-w** per esborrar l'última paraula i **ctrl-u** per esborrar el contingut des del cursor actual fins a l'inici de la línia. Utilitzeu **alt-b** i **alt-f** per moure's per paraula, **ctrl-a** per moure el cursor al principi de la línia, **ctrl-e** per moure el cursor al final de la línia , **ctrl-k** per matar fins al final de la línia, **ctrl-l** per esborrar la pantalla. Vegeu `man readline` per a totes les combinacions de tecles predeterminades a Bash. Hi ha molts. Per exemple, **alt-.** fa un cicle a través dels arguments anteriors i **alt-*** expandeix un globus.


- Alternativament, si us agraden les combinacions de tecles d'estil vi, utilitzeu `set -o vi` (i `set -o emacs` per tornar-lo a posar).

- Per editar ordres llargues, després de configurar el vostre editor (per exemple, `export EDITOR=vim`), **ctrl-x** **ctrl-e** obrirà l'ordre actual en un editor per a l'edició de diverses línies. O a l'estil vi, **escape-v**.

- Per veure les ordres recents, utilitzeu `historial`. Seguiu amb `!n` (on `n` és el número d'ordre) per executar-lo de nou. També hi ha moltes abreviatures que podeu utilitzar, la més útil probablement sigui `!$` per a l'últim argument i `!!` per a l'última ordre (vegeu "EXPANSIÓ DE LA HISTÒRIA" a la pàgina de manual). Tanmateix, sovint es substitueixen fàcilment per **ctrl-r** i **alt-.**.

- Aneu al vostre directori d'inici amb `cd`. Accediu als fitxers relatius al vostre directori d'inici amb el prefix `~` (per exemple, `~/.bashrc`). En els scripts `sh` es refereixen al directori inicial com a `$HOME`.

- Per tornar al directori de treball anterior: `cd -`.

- Si esteu a la meitat d'escriure una ordre però canvieu d'opinió, premeu **alt-#** per afegir un `#` al principi i introduïu-lo com a comentari (o feu servir **ctrl-a**, ** #**, **entra**). A continuació, podeu tornar-hi més tard mitjançant l'historial d'ordres.

- Utilitzeu `xargs` (o `paral·lel`). És molt potent. Tingueu en compte que podeu controlar quants elements s'executen per línia (`-L`) així com el paral·lelisme (`-P`). Si no esteu segur de si farà el correcte, utilitzeu primer `xargs echo`. A més, `-I{}` és útil. Exemples:
```bash
      trobar. -nom '*.py' | xargs grep some_function
      hostes de gats | xargs -I{} ssh root@{} nom d'amfitrió
```

- `pstree -p` és una visualització útil de l'arbre de processos.

- Utilitzeu `pgrep` i `pkill` per trobar o senyalitzar processos pel nom (`-f` és útil).

- Coneix els diferents senyals que pots enviar als processos. Per exemple, per suspendre un procés, utilitzeu `kill -STOP [pid]`. Per a la llista completa, vegeu "senyal home 7".

- Utilitzeu `nohup` o `disown` si voleu que un procés en segon pla es continuï executant per sempre.

- Comproveu quins processos escolten mitjançant `netstat -lntp` o `ss -plat` (per a TCP; afegiu `-u` per a UDP) o `lsof -iTCP -sTCP:LISTEN -P -n` (que també funciona a macOS ).

- Vegeu també `lsof` i `fuser` per a sockets i fitxers oberts.

- Vegeu `uptime` o `w` per saber quant de temps s'està executant el sistema.

- Utilitzeu "àlies" per crear dreceres per a les ordres d'ús habitual. Per exemple, `alias ll='ls -latr'` crea un nou àlies `ll`.

- Deseu els àlies, la configuració de l'intèrpret d'ordres i les funcions que utilitzeu habitualment a `~/.bashrc`, i [organitzeu que els intèrprets d'ordres d'inici de sessió se'n derivin] (http://superuser.com/a/183980/7106). Això farà que la vostra configuració estigui disponible en totes les vostres sessions de shell.

- Posa la configuració de les variables d'entorn, així com les ordres que s'han d'executar quan inicies sessió a `~/.bash_profile`. Es necessitarà una configuració separada per als intèrprets d'ordres que inicieu des dels inicis de sessió de l'entorn gràfic i els treballs "cron".

- Sincronitzeu els vostres fitxers de configuració (per exemple, `.bashrc` i `.bash_profile`) entre diversos ordinadors amb Git.

- Entendre que cal tenir cura quan les variables i els noms de fitxer inclouen espais en blanc. Envolta les variables Bash amb cometes, p. `"$FOO"`. Preferiu les opcions `-0` o `-print0` per habilitar caràcters nuls per delimitar els noms de fitxers, p. `localitza el patró -0 | xargs -0 ls -al` o `find / -print0 -type d | xargs -0 ls -al`. Per iterar sobre noms de fitxer que contenen espais en blanc en un bucle for, configureu el vostre IFS perquè sigui una nova línia només utilitzant `IFS=$'\n'`.

- En els scripts de Bash, utilitzeu `set -x` (o la variant `set -v`, que registra l'entrada en brut, incloses variables i comentaris no ampliats) per a la depuració de la sortida. Utilitzeu modes estrictes tret que tingueu una bona raó per no fer-ho: Utilitzeu `set -e` per avortar els errors (codi de sortida diferent de zero). Utilitzeu `set -u` per detectar els usos de variables no configurats. Considereu també `set -o pipefail` per avortar els errors dins de les canonades (tot i que llegiu-ne més si ho feu, ja que aquest tema és una mica subtil). Per a scripts més implicats, utilitzeu també "trap" a EXIT o ERR. Un hàbit útil és iniciar un script com aquest, que el farà detectar i avortar els errors habituals i imprimir un missatge:
```bash
      set -euo pipefail
      trap "echo 'error: Script failed: vege failed command above'" ERR
```

- En els scripts Bash, les subshells (escriudes amb parèntesis) són maneres convenients d'agrupar ordres. Un exemple comú és moure's temporalment a un directori de treball diferent, p.
```bash
      # fer alguna cosa al directori actual
      (cd /some/other/dir && other-command)
      # continua amb el directori original
```
- A Bash, tingueu en compte que hi ha molts tipus d'expansió variable. Comprovant que existeix una variable: `${name:?error message}`. Per exemple, si un script Bash requereix un sol argument, només cal que escriviu `input_file=${1:?usage: $0 input_file}`. Utilitzant un valor predeterminat si una variable està buida: `${name:-default}`. Si voleu afegir un paràmetre addicional (opcional) a l'exemple anterior, podeu utilitzar alguna cosa com `output_file=${2:-logfile}`. Si `$2` s'omet i, per tant, està buit, `output_file` s'establirà com a `logfile`. Expansió aritmètica: `i=$(( (i + 1) % 5 ))`. Seqüències: `{1..10}`. Retall de cadenes: `${var%suffix}` i `${var#prefix}`. Per exemple, si `var=foo.pdf`, aleshores `echo ${var%.pdf}.txt` imprimeix `foo.txt`.

- L'expansió de claus utilitzant `{`...`}` pot reduir haver de tornar a escriure text similar i automatitzar les combinacions d'elements. Això és útil en exemples com `mv foo.{txt,pdf} some-dir` (que mou els dos fitxers), `cp somefile{,.bak}` (que s'amplia a `cp somefile somefile.bak`) o `mkdir -p prova-{a,b,c}/subtest-{1,2,3}` (que amplia totes les combinacions possibles i crea un arbre de directoris). L'expansió de tirants es realitza abans de qualsevol altra expansió.

- L'ordre de les expansions és: expansió de tirants; expansió de tilde, expansió de paràmetres i variables, expansió aritmètica i substitució d'ordres (s'ha fet d'esquerra a dreta); divisió de paraules; i l'ampliació del nom de fitxer. (Per exemple, un interval com `{1..20}` no es pot expressar amb variables utilitzant `{$a..$b}`. Utilitzeu `seq` o un bucle `for`, per exemple, `seq $a $b` o `for((i=a; i<=b; i++)); do ... ; fet`.)

- La sortida d'una ordre es pot tractar com un fitxer mitjançant `<(alguna ordre)` (coneguda com a substitució de processos). Per exemple, compareu `/etc/hosts` local amb un de remot:
```sh
      diff /etc/hosts <(ssh somehost cat /etc/hosts)
```

- Quan escriviu scripts, és possible que vulgueu posar tot el vostre codi entre claus. Si falta la clau de tancament, el vostre script no s'executarà a causa d'un error de sintaxi. Això té sentit quan el vostre script es baixarà del web, ja que impedeix que s'executin scripts parcialment baixats:
```bash
{
      # El teu codi aquí
}
```

- Un "document aquí" permet [la redirecció de múltiples línies d'entrada](https://www.tldp.org/LDP/abs/html/here-docs.html) com si fos d'un fitxer:
```
gat <<EOF
entrada
en múltiples línies
EOF
```

- A Bash, redirigeix ​​tant la sortida estàndard com l'error estàndard mitjançant: `some-command >logfile 2>&1` o `some-command &>logfile`. Sovint, per assegurar-vos que una ordre no deixi un identificador de fitxer obert a l'entrada estàndard, lligant-lo al terminal on us trobeu, també és una bona pràctica afegir `</dev/null`.

- Utilitzeu `man ascii` per a una bona taula ASCII, amb valors hexadecimals i decimals. Per a informació general de codificació, `man unicode`, `man utf-8` i `man latin1` són útils.

- Utilitzeu `screen` o [`tmux`](https://tmux.github.io/) per multiplexar la pantalla, especialment útil en sessions ssh remotes i per desconnectar i tornar a connectar-hi una sessió. `byobu` pot millorar la pantalla o tmux proporcionant més informació i una gestió més fàcil. Una alternativa més mínima només per a la persistència de la sessió és [`dtach`](https://github.com/bogner/dtach).

- En ssh, saber com portar el túnel amb `-L` o `-D` (i ocasionalment `-R`) és útil, p. per accedir a llocs web des d'un servidor remot.

- Pot ser útil fer algunes optimitzacions a la configuració de ssh; per exemple, aquest `~/.ssh/config` conté paràmetres per evitar connexions caigudes en determinats entorns de xarxa, utilitza compressió (que és útil amb scp en connexions de baix ample de banda) i multiplexa canals al mateix servidor amb un fitxer de control local :
```
      TPCKeepAlive=sí
      ServerAliveInterval=15
      ServerAliveCountMax=6
      Compressió=sí
      ControlMaster automàtic
      ControlPath /tmp/%r@%h:%p
      ControlPersist sí
```

- Algunes altres opcions rellevants per a ssh són sensibles a la seguretat i s'han d'habilitar amb cura, p. per subxarxa o host o en xarxes de confiança: `StrictHostKeyChecking=no`, `ForwardAgent=yes`

- Considereu [`mosh`](https://mosh.mit.edu/) una alternativa a ssh que utilitza UDP, evitant connexions caigudes i afegint comoditat a la carretera (requereix una configuració del servidor).

- Per obtenir els permisos d'un fitxer en forma octal, que és útil per a la configuració del sistema, però no està disponible en `ls` i fàcil d'efectuar, utilitzeu alguna cosa com
```sh
stat -c '%A %a %n' /etc/timezone
```

- Per a la selecció interactiva de valors de la sortida d'una altra ordre, utilitzeu [`percol`](https://github.com/mooz/percol) o [`fzf`](https://github.com/junegunn/fzf ).

- Per a la interacció amb fitxers basats en la sortida d'una altra ordre (com ara `git`), utilitzeu `fpp` ([PathPicker](https://github.com/facebook/PathPicker)).

- Per a un servidor web senzill per a tots els fitxers del directori actual (i subdir), disponible per a qualsevol persona de la vostra xarxa, utilitzeu:
`python -m SimpleHTTPServer 7777` (per al port 7777 i Python 2) i `python -m http.server 7777` (per al port 7777 i Python 3).

- Per executar una ordre com un altre usuari, utilitzeu `sudo`. Per defecte, s'executa com a root; utilitzeu `-u` per especificar un altre usuari. Utilitzeu `-i` per iniciar sessió com a aquest usuari (se us demanarà la _la vostra_ contrasenya).

- Per canviar l'intèrpret d'ordres a un altre usuari, utilitzeu `su username` o `su - username`. Aquest últim amb "-" obté un entorn com si un altre usuari acabés d'iniciar sessió. Si ometem el nom d'usuari, el valor predeterminat és root. Se us demanarà la contrasenya _de l'usuari al qual canvieu_.

- Coneixeu el [límit de 128K](https://wiki.debian.org/CommonErrorMessages/ArgumentListTooLong) a les línies d'ordres. Aquest error "Llista d'arguments massa llarga" és comú quan el comodí coincideix amb un gran nombre de fitxers. (Quan això succeeix, alternatives com "find" i "xargs" poden ajudar.)

- Per a una calculadora bàsica (i, per descomptat, accés a Python en general), utilitzeu l'intèrpret `python`. Per exemple,
```
>>> 2+3
5
```


## Processament de fitxers i dades

- Per localitzar un fitxer pel nom al directori actual, `find . -iname '*alguna cosa*'' (o similar). Per trobar un fitxer a qualsevol lloc pel seu nom, utilitzeu `localitza alguna cosa` (però tingueu en compte que `updatedb` pot no haver indexat els fitxers creats recentment).

- Per a la cerca general a través de fitxers d'origen o de dades, hi ha diverses opcions més avançades o més ràpides que `grep -r`, incloent (en ordre aproximat de més antic a més recent) [`ack`](https://github.com/beyondgrep /ack2), [`ag`](https://github.com/ggreer/the_silver_searcher) ("el cercador de plata") i [`rg`](https://github.com/BurntSushi/ripgrep) ( ripgrep).
- Per convertir HTML a text: `lynx -dump -stdin`

- Per a Markdown, HTML i tot tipus de conversió de documents, proveu [`pandoc`](http://pandoc.org/). Per exemple, per convertir un document Markdown a format Word: `pandoc README.md --from markdown --to docx -o temp.docx`

- Si heu de gestionar XML, `xmlstarlet` és antic però bo.

- Per a JSON, utilitzeu [`jq`](http://stedolan.github.io/jq/). Per a un ús interactiu, vegeu també [`jid`](https://github.com/simeji/jid) i [`jiq`](https://github.com/fiatjaf/jiq).

- Per a YAML, utilitzeu [`shyaml`](https://github.com/0k/shyaml).

- Per a fitxers Excel o CSV, [csvkit](https://github.com/onyxfish/csvkit) proporciona `in2csv`, `csvcut`, `csvjoin`, `csvgrep`, etc.

- Per a Amazon S3, [`s3cmd`](https://github.com/s3tools/s3cmd) és convenient i [`s4cmd`](https://github.com/bloomreach/s4cmd) és més ràpid. [`aws`](https://github.com/aws/aws-cli) d'Amazon i [`saws`](https://github.com/donnemartin/saws) millorats són essencials per a altres tasques relacionades amb AWS .

- Conegui `sort` i `uniq`, incloses les opcions `-u` i `-d` d'uniq -- vegeu les línies a continuació. Vegeu també `comm`.

- Conegui `tallar`, `enganxar` i `unir` per manipular fitxers de text. Molta gent fa servir "tallar", però s'obliden de "unir-se".

- Saber sobre `wc` per comptar noves línies (`-l`), caràcters (`-m`), paraules (`-w`) i bytes (`-c`).

- Conèixer `tee` per copiar de stdin a un fitxer i també a stdout, com a `ls -al | tee file.txt`.

- Per a càlculs més complexos, com ara l'agrupació, els camps invertits i els càlculs estadístics, considereu [`datamash`](https://www.gnu.org/software/datamash/).

- Sapigueu que la configuració regional afecta moltes eines de línia d'ordres de maneres subtils, inclòs l'ordre d'ordenació (intercalació) i el rendiment. La majoria de les instal·lacions de Linux establiran `LANG' o altres variables locals amb una configuració local com l'anglès dels EUA. Però tingueu en compte que l'ordenació canviarà si canvieu la configuració regional. I sabeu que les rutines i18n poden fer que l'ordenació o altres ordres s'executin *moltes vegades* més lenta. En algunes situacions (com les operacions de conjunt o les operacions d'unicitat a continuació) podeu ignorar completament les rutines i18n lentes i utilitzar l'ordre d'ordenació tradicional basat en bytes, utilitzant `export LC_ALL=C`.

- Podeu configurar l'entorn d'una ordre específica prefixant la seva invocació amb la configuració de la variable d'entorn, com a `TZ=Pacific/Fiji date`.

- Conèixer `awk` i `sed` bàsics per obtenir dades senzilles. Vegeu [One-liners](#one-liners) per obtenir exemples.

- Per substituir totes les ocurrències d'una cadena al seu lloc, en un o més fitxers:
```sh
      perl -pi.bak -e 's/old-string/new-string/g' my-files-*.txt
```

- Per canviar el nom de diversos fitxers i/o cercar i substituir-hi fitxers, proveu [`repren`](https://github.com/jlevy/repren). (En alguns casos, l'ordre `canviar nom` també permet canvis de nom, però aneu amb compte ja que la seva funcionalitat no és la mateixa a totes les distribucions de Linux.)
```sh
      # Canvi de nom complet dels noms de fitxers, directoris i contingut foo -> bar:
      repren --full --preserve-case --de foo --a bar.
      # Recuperar fitxers de còpia de seguretat whatever.bak -> whatever:
      repren --renames --de '(.*)\.bak' --a '\1' *.bak
      # Igual que l'anterior, fent servir canviar el nom, si està disponible:
      canviar el nom de 's/\.bak$//' *.bak
```

- Com diu la pàgina de manual, `rsync` és realment una eina de còpia de fitxers ràpida i extraordinàriament versàtil. És conegut per sincronitzar-se entre màquines, però és igualment útil a nivell local. Quan les restriccions de seguretat ho permeten, utilitzar `rsync` en lloc de `scp` permet recuperar una transferència sense reiniciar des de zero. També es troba entre les [maneres més ràpides](https://web.archive.org/web/20130929001850/http://linuxnote.net/jianingy/en/linux/a-fast-way-to-remove-huge- number-of-files.html) per eliminar un gran nombre de fitxers:
```sh
mkdir buit && rsync -r --eliminar buit/ some-dir && rmdir some-dir
```

- Per controlar el progrés en processar fitxers, utilitzeu [`pv`](http://www.ivarch.com/programs/pv.shtml), [`pycp`](https://github.com/dmerejkowsky/pycp) , [`pmonitor`](https://github.com/dspinellis/pmonitor), [`progress`](https://github.com/Xfennec/progress), `rsync --progress` o, per al bloc -nivell de còpia, `dd status=progress`.

- Utilitzeu `shuf` per barrejar o seleccionar línies aleatòries d'un fitxer.

- Conèixer les opcions de `ordenar`. Per als números, utilitzeu `-n`, o `-h` per manejar números llegibles per humans (p. ex., de `du -h`). Saber com funcionen les tecles (`-t` i `-k`). En particular, tingueu en compte que heu d'escriure `-k1,1` per ordenar només pel primer camp; `-k1` significa ordenar segons la línia sencera. L'ordenació estable ('sort -s') pot ser útil. Per exemple, per ordenar primer pel camp 2 i, després, secundàriament pel camp 1, podeu utilitzar `sort -k1,1 | ordena -s -k2,2`.

- Si mai necessiteu escriure un literal de tabulació en una línia d'ordres a Bash (per exemple, per ordenar l'argument -t), premeu **ctrl-v** **[Tab]** o escriviu `$'\t' ` (aquest últim és millor, ja que el podeu copiar/enganxar).

- Les eines estàndard per aplicar pedaços al codi font són `diff` i `patch`. Vegeu també `diffstat` per a les estadístiques de resum d'una diferència i `sdiff` per a una diferència de costat a costat. Tingueu en compte que `diff -r` funciona per a directoris sencers. Utilitzeu `diff -r tree1 tree2 | diffstat` per obtenir un resum dels canvis. Utilitzeu `vimdiff` per comparar i editar fitxers.

- Per als fitxers binaris, utilitzeu `hd`, `hexdump` o `xxd` per a abocaments hexadecimals simples i `bvi`, `hexedit` o `biew` per a l'edició binària.

- També per als fitxers binaris, `cadenes` (més `grep`, etc.) us permet trobar fragments de text.

- Per a les diferències binàries (compressió delta), utilitzeu `xdelta3`.

- Per convertir codificacions de text, proveu `iconv`. O `uconv` per a un ús més avançat; és compatible amb algunes coses Unicode avançades. Per exemple:
```sh
      # Mostra codis hexadecimals o noms reals de caràcters (útils per a la depuració):
      uconv -f utf-8 -t utf-8 -x '::Any-Hex;' < input.txt
      uconv -f utf-8 -t utf-8 -x '::Any-Name;' < input.txt
      # Minúscules i elimina tots els accents (ampliant-los i deixant-los anar):
      uconv -f utf-8 -t utf-8 -x '::Any-Inferior; ::Qualsevol-NFD; [:Marca sense espai:] >; ::Qualsevol-NFC;' < input.txt > output.txt
```

- Per dividir fitxers en trossos, vegeu `split` (per dividir per mida) i `csplit` (per dividir per un patró).

- Data i hora: per obtenir la data i l'hora actuals en el format útil [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601), utilitzeu `date -u +"%Y-%m-% dT%H:%M:%SZ"` (altres opcions [són](https://stackoverflow.com/questions/7216358/date-command-on-os-x-doesnt-have-iso-8601-i- opció) [problemàtic](https://unix.stackexchange.com/questions/164826/date-command-iso-8601-option)). Per manipular expressions de data i hora, utilitzeu `dateadd`, `datediff`, `strptime`, etc. de [`dateutils`](http://www.fresse.org/dateutils/).

- Utilitzeu `zless`, `zmore`, `zcat` i `zgrep` per operar amb fitxers comprimits.

- Els atributs dels fitxers es poden configurar mitjançant `chattr` i ofereixen una alternativa de nivell inferior als permisos de fitxers. Per exemple, per protegir-se de la supressió accidental de fitxers, el senyalador immutable: `sudo chattr +i /critical/directory/or/file`

- Utilitzeu `getfacl` i `setfacl` per desar i restaurar els permisos dels fitxers. Per exemple:
```sh
getfacl -R /some/path > permissions.txt
   setfacl --restore=permissions.txt
```

- Per crear fitxers buits ràpidament, utilitzeu `truncate` (crea [fitxer dispers](https://en.wikipedia.org/wiki/Sparse_file)), `fallocate` (sistemes de fitxers ext4, xfs, btrfs i ocfs2), `xfs_mkfile ` (gairebé qualsevol sistema de fitxers, ve al paquet xfsprogs), `mkfile' (per a sistemes semblants a Unix com Solaris, Mac OS).

## Depuració del sistema

- Per a la depuració web, `curl` i `curl -I` són útils, o els seus equivalents `wget`, o els més moderns [`httpie`](https://github.com/jkbrzt/httpie).

- Per conèixer l'estat actual de la CPU/disc, les eines clàssiques són `top` (o millor `htop`), `iostat` i `iotop`. Utilitzeu `iostat -mxz 15` per a la CPU bàsica i les estadístiques detallades del disc per partició i la visió del rendiment.

- Per als detalls de la connexió de xarxa, utilitzeu `netstat` i `ss`.

- Per a una visió general ràpida del que està passant en un sistema, `dstat` és especialment útil. Per obtenir una visió general més àmplia amb detalls, utilitzeu [`glances`](https://github.com/nicolargo/glances).

- Per conèixer l'estat de la memòria, executar i comprendre la sortida de `free` i `vmstat`. En particular, tingueu en compte que el valor "emmagatzemat en memòria cau" és la memòria que manté el nucli de Linux com a memòria cau de fitxers, de manera que compta efectivament per al valor "lliure".

- La depuració del sistema Java és una altra cosa, però un truc senzill a Oracle i algunes altres JVM és que podeu executar `kill -3 <pid>` i un seguiment complet de la pila i un resum del munt (inclosos els detalls de la recollida d'escombraries generacionals, que pot ser molt informatiu) s'enviarà a stderr/logs. Els `jps`, `jstat`, `jstack`, `jmap` del JDK són útils. [Les eines SJK](https://github.com/aragozin/jvm-tools) són més avançades.

- Utilitzeu [`mtr`](http://www.bitwizard.nl/mtr/) com a millor traceroute, per identificar problemes de xarxa.

- Per veure per què un disc està ple, [`ncdu`](https://dev.yorhel.nl/ncdu) estalvia temps amb les ordres habituals com `du -sh *`.

- Per trobar quin sòcol o procés utilitza ample de banda, proveu [`iftop`](http://www.ex-parrot.com/~pdw/iftop/) o [`nethogs`](https://github.com /raboof/nethogs).

- L'eina `ab` (vé amb Apache) és útil per a la comprovació ràpida i bruta del rendiment del servidor web. Per a proves de càrrega més complexes, proveu `siege`.
- Per a una depuració de xarxa més seriosa, [`wireshark`](https://wireshark.org/), [`tshark`](https://www.wireshark.org/docs/wsug_html_chunked/AppToolstshark.html) o [ `ngrep`](http://ngrep.sourceforge.net/).

- Conèixer `trace` i `ltrace`. Aquests poden ser útils si un programa falla, es penja o falla, i no sabeu per què, o si voleu tenir una idea general del rendiment. Tingueu en compte l'opció de creació de perfils (`-c`) i la possibilitat d'adjuntar-se a un procés en execució (`-p`). Utilitzeu l'opció de rastreig fill (`-f`) per evitar trucades importants perdudes.

- Coneixeu `ldd` per comprovar biblioteques compartides, etc., però [mai l'executeu en fitxers no fiables] (http://www.catonmat.net/blog/ldd-arbitrary-code-execution/).

- Saber connectar-se a un procés en execució amb `gdb` i obtenir els seus rastres de pila.

- Utilitzeu `/proc`. De vegades és increïblement útil quan es depuren problemes en directe. Exemples: `/proc/cpuinfo`, `/proc/meminfo`, `/proc/cmdline`, `/proc/xxx/cwd`, ​​`/proc/xxx/exe`, `/proc/xxx/fd/` , `/proc/xxx/smaps` (on `xxx` és l'identificador o pid del procés).

- En depurar per què alguna cosa va fallar en el passat, [`sar`](http://sebastien.godard.pagesperso-orange.fr/) pot ser molt útil. Mostra estadístiques històriques sobre CPU, memòria, xarxa, etc.

- Per a sistemes més profunds i anàlisis de rendiment, consulteu `stap` ([SystemTap](https://sourceware.org/systemtap/wiki)), [`perf`](https://en.wikipedia.org/wiki/ Perf_%28Linux%29) i [`sysdig`](https://github.com/draios/sysdig).

- Comproveu quin sistema operatiu esteu amb `uname` o `uname -a` (informació general d'Unix/kernel) o `lsb_release -a` (informació de distribució de Linux).

- Utilitzeu `dmesg` sempre que alguna cosa faci molt de gràcia (podrien ser problemes de maquinari o de controlador).

- Si suprimiu un fitxer i no allibera espai de disc esperat tal com informa `du`, comproveu si el fitxer està en ús per un procés:
`lsof | grep suprimit | grep "nom-fitxer-del-meu-fitxer-gran"`


## Una línia

Alguns exemples d'ordres d'unió:

- De vegades és molt útil que pugueu establir la intersecció, la unió i la diferència de fitxers de text mitjançant `sort`/`uniq`. Suposem que "a" i "b" són fitxers de text que ja estan únics. Això és ràpid i funciona amb fitxers de mida arbitrària, de fins a molts gigabytes. (L'ordenació no està limitada per la memòria, tot i que és possible que hàgiu d'utilitzar l'opció `-T` si `/tmp` es troba en una partició arrel petita.) Vegeu també la nota sobre `LC_ALL` anterior i `sort`'s `- opció u` (oblida per a més claredat a continuació).

```sh
      ordena a b | uniq > c # c és una unió b
      ordena a b | uniq -d > c # c és una intersecció b
      ordena a b b | uniq -u > c # c és la diferència establerta a - b
```

- Imprimiu dos fitxers JSON, normalitzeu-ne la sintaxi i, a continuació, acolorieu i paginau el resultat:
```
      diff <(jq --sort-keys . < fitxer1.json) <(jq --sort-keys . < fitxer2.json) | colordiff | menys -R
```

- Utilitzeu `grep. *` per examinar ràpidament el contingut de tots els fitxers d'un directori (de manera que cada línia s'acobla amb el nom del fitxer), o `head -100 *` (de manera que cada fitxer tingui un encapçalament). Això pot ser útil per a directoris plens de paràmetres de configuració com els de `/sys`, `/proc`, `/etc`.


- Suma de tots els números de la tercera columna d'un fitxer de text (això és probablement 3 vegades més ràpid i 3 vegades menys codi que l'equivalent Python):
```sh
      awk '{ x += $3 } END { print x }' el meu fitxer
```

- Per veure les mides/dates en un arbre de fitxers, això és com un recursiu `ls -l` però és més fàcil de llegir que `ls -lR`:
```sh
      trobar. -tipus f -ls
```

- Suposem que teniu un fitxer de text, com ara un registre de servidor web, i un valor determinat que apareix en algunes línies, com ara un paràmetre `acct_id` present a l'URL. Si voleu un recompte de quantes sol·licituds per a cada "acct_id":
```sh
      egrep -o 'acct_id=[0-9]+' access.log | tallar -d= -f2 | ordenar | uniq -c | ordenar -rn
```

- Per controlar contínuament els canvis, utilitzeu "rellotge", p. comproveu els canvis als fitxers d'un directori amb `watch -d -n 2 'ls -rtlh | tail'` o a la configuració de la xarxa mentre resoleu problemes de configuració de wifi amb `watch -d -n 2 ifconfig`.

- Executeu aquesta funció per obtenir un consell aleatori d'aquest document (analitza Markdown i extreu un element):
```sh
      funció taocl() {
        curl -s https://raw.githubusercontent.com/jlevy/the-art-of-command-line/master/README.md |
          sed '/cowsay[.]png/d' |
          pandoc -f markdown -t html |
          xmlstarlet per a --html --dropdtd |
          xmlstarlet sel -t -v "(html/body/ul/li[count(p)>0])[$RANDOM mod last()+1]" |
          xmlstarlet unesc | fmt -80 | iconv -t EUA
      }
```


## Obscur però útil

- `expr`: realitza operacions aritmètiques o booleanes o avalua expressions regulars

- `m4`: processador de macro simple

- `sí`: imprimir una cadena molt

- `cal`: bonic calendari

- `env`: executa una ordre (útil en scripts)

- `printenv`: imprimeix variables d'entorn (útil en depuració i scripts)

- `look`: cerca paraules en anglès (o línies en un fitxer) que comencen amb una cadena

- `tallar`, `enganxar` i `unir`: manipulació de dades

- `fmt`: format dels paràgrafs de text

- `pr`: donar format al text en pàgines/columnes

- `plega': embolcalla línies de text

- `columna`: donar format als camps de text en columnes o taules alineades i d'amplada fixa

- `expand` i `unexpand`: converteix entre tabulacions i espais

- `nl`: afegeix números de línia

- `seq`: imprimir números

- `bc`: calculadora

- `factor`: factor enters

- [`gpg`](https://gnupg.org/): xifrar i signar fitxers

- `toe`: taula d'entrades de terminfo

- `nc`: depuració de la xarxa i transferència de dades

- `socat`: relé de socket i reenviador de ports tcp (similar a `netcat`)

- [`slurm`](https://github.com/mattthias/slurm): visualització del trànsit de xarxa

- `dd`: moure dades entre fitxers o dispositius
- `fitxer`: identifica el tipus d'un fitxer

- `tree`: mostra directoris i subdirectoris com un arbre de nidificació; com `ls` però recursiu

- `stat`: informació del fitxer

- `time`: executa i cronometra una ordre

- `timeout`: executeu una ordre durant un període de temps especificat i atureu el procés quan finalitzi el temps especificat.

- `lockfile`: crea un fitxer de semàfor que només es pot eliminar amb `rm -f`

- `logrotate`: gira, comprimeix i envia els registres.

- `watch`: executa una ordre repetidament, mostrant resultats i/o ressaltant els canvis

- [`when-changed`](https://github.com/joh/when-changed): executa qualsevol ordre que especifiqueu sempre que veu el fitxer canviat. Vegeu també `inotifywait` i `entr`.

- `tac`: imprimeix fitxers al revés

- `comm`: compara els fitxers ordenats línia per línia

- `cadenes`: extreu text dels fitxers binaris

- `tr`: traducció o manipulació de caràcters

- `iconv` o `uconv`: conversió per a codificacions de text

- `split` i `csplit`: dividir fitxers

- `sponge`: llegeix tota l'entrada abans d'escriure-la, útil per llegir des d'escriure al mateix fitxer, per exemple, `grep -v alguna cosa algun fitxer | esponja algun fitxer'

- "unitats": càlculs i conversions d'unitats; converteix furlongs per quinze dies a twips per parpelleig (vegeu també `/usr/share/units/definitions.units`)

- `apg`: genera contrasenyes aleatòries

- `xz`: compressió de fitxers d'alta proporció

- `ldd`: informació dinàmica de la biblioteca

- `nm`: símbols dels fitxers d'objectes

- `ab` o [`wrk`](https://github.com/wg/wrk): comparació de servidors web

- `strace`: depuració de trucades del sistema

- [`mtr`](http://www.bitwizard.nl/mtr/): millor traceroute per a la depuració de xarxes

- `cssh`: shell visual concurrent

- `rsync`: sincronitza fitxers i carpetes mitjançant SSH o en el sistema de fitxers local

- [`wireshark`](https://wireshark.org/) i [`tshark`](https://www.wireshark.org/docs/wsug_html_chunked/AppToolstshark.html): captura de paquets i depuració de xarxes

- [`ngrep`](http://ngrep.sourceforge.net/): grep per a la capa de xarxa

- `host` i `dig`: cerques de DNS

- `lsof`: descriptor del fitxer de procés i informació del sòcol

- `dstat`: estadístiques útils del sistema

- [`glances`](https://github.com/nicolargo/glances): visió general de diversos subsistemes d'alt nivell

- `iostat`: estadístiques d'ús del disc

- `mpstat`: estadístiques d'ús de la CPU

- `vmstat`: estadístiques d'ús de la memòria

- `htop`: versió millorada de la part superior

- `últim`: historial d'inici de sessió

- `w`: qui ha iniciat sessió

- `id`: informació d'identitat d'usuari/grup

- [`sar`](http://sebastien.godard.pagesperso-orange.fr/): estadístiques històriques del sistema

- [`iftop`](http://www.ex-parrot.com/~pdw/iftop/) o [`nethogs`](https://github.com/raboof/nethogs): utilització de la xarxa per sòcol o procés

- `ss`: estadístiques de socket

- `dmesg`: missatges d'error del sistema i d'arrencada

- `sysctl`: visualitza i configura els paràmetres del nucli de Linux en temps d'execució

- `hdparm`: manipulació/rendiment del disc SATA/ATA

- `lsblk`: llista de dispositius de bloc: una vista en arbre dels vostres discs i particions de disc

- `lshw`, `lscpu`, `lspci`, `lsusb`, `dmidecode`: informació de maquinari, incloent CPU, BIOS, RAID, gràfics, dispositius, etc.

- `lsmod` i `modinfo`: Llista i mostra els detalls dels mòduls del nucli.

- `fortune`, `ddate` i `sl`: um, bé, depèn de si considereu que les locomotores de vapor i les cites de Zippy són "útils"


## només macOS

Aquests són elements rellevants *només* a macOS.

- Gestió de paquets amb `brew` (Homebrew) i/o `port` (MacPorts). Es poden utilitzar per instal·lar a macOS moltes de les ordres anteriors.

- Copieu la sortida de qualsevol comanda a una aplicació d'escriptori amb "pbcopy" i enganxeu l'entrada d'una amb "pbpaste".

- Per habilitar la tecla d'opció a macOS Terminal com a tecla alt (com s'utilitza a les ordres anteriors com **alt-b**, **alt-f**, etc.), obriu Preferències -> Perfils -> Teclat i seleccioneu "Utilitza l'opció com a clau meta".

- Per obrir un fitxer amb una aplicació d'escriptori, utilitzeu `open` o `open -a /Applications/Whatever.app`.

- Spotlight: cerqueu fitxers amb "mdfind" i metadades de llista (com ara informació EXIF ​​de fotos) amb "mdls".

- Tingueu en compte que macOS es basa en BSD Unix i moltes ordres (per exemple `ps`, `ls`, `tail`, `awk`, `sed`) tenen moltes variacions subtils de Linux, que està molt influenciada pel System V. Eines d'estil Unix i GNU. Sovint podeu notar la diferència observant que una pàgina de manual té l'encapçalament "Manual d'ordres generals de BSD". En alguns casos també es poden instal·lar versions de GNU (com ara `gawk` i `gsed` per a GNU awk i sed). Si escriviu scripts Bash multiplataforma, eviteu aquestes ordres (per exemple, considereu Python o `perl`) o proveu amb cura.

- Per obtenir informació de llançament de macOS, utilitzeu `sw_vers`.

## Només Windows

Aquests elements són rellevants *només* a Windows.

### Maneres d'obtenir eines Unix a Windows

- Accediu al poder de l'intèrpret d'ordres Unix a Microsoft Windows instal·lant [Cygwin](https://cygwin.com/). La majoria de les coses descrites en aquest document funcionaran fora de la caixa.

- A Windows 10, podeu utilitzar [Windows Subsystem for Linux (WSL)](https://msdn.microsoft.com/commandline/wsl/about), que proporciona un entorn Bash familiar amb utilitats de línia d'ordres Unix.

- Si voleu utilitzar principalment les eines de desenvolupament de GNU (com ara GCC) a Windows, considereu [MinGW](http://www.mingw.org/) i el seu [MSYS](http://www.mingw.org/). wiki/msys), que proporciona utilitats com bash, gawk, make i grep. MSYS no té totes les característiques en comparació amb Cygwin. MinGW és especialment útil per crear ports natius de Windows d'eines Unix.

- Una altra opció per obtenir l'aspecte i la sensació d'Unix a Windows és [Efectiu] (https://github.com/dthree/cash). Tingueu en compte que només hi ha molt poques ordres Unix i opcions de línia d'ordres disponibles en aquest entorn.

### Eines útils de línia d'ordres de Windows

- Podeu realitzar i programar la majoria de tasques d'administració del sistema de Windows des de la línia d'ordres aprenent i utilitzant `wmic`.

- Les eines de xarxa natives de la línia d'ordres de Windows que podeu trobar útils inclouen `ping`, `ipconfig`, `tracert` i `netstat`.

- Podeu realitzar [moltes tasques útils de Windows](http://www.thewindowsclub.com/rundll32-shortcut-commands-windows) invocant l'ordre `Rundll32`.

### Consells i trucs de Cygwin

- Instal·leu programes Unix addicionals amb el gestor de paquets de Cygwin.

- Utilitzeu `mintty` com a finestra de línia d'ordres.

- Accediu al porta-retalls de Windows mitjançant `/dev/clipboard`.

- Executeu `cygstart` per obrir un fitxer arbitrari mitjançant la seva aplicació registrada.

- Accediu al registre de Windows amb `regtool`.

- Tingueu en compte que una ruta de la unitat de Windows `C:\` es converteix en `/cygdrive/c` a Cygwin, i que `/` de Cygwin apareix sota `C:\cygwin` a Windows. Converteix entre les rutes de fitxers d'estil Cygwin i Windows amb `cygpath`. Això és molt útil en els scripts que invoquen programes de Windows.

## Més recursos

- [awesome-shell](https://github.com/alebcay/awesome-shell): una llista seleccionada d'eines i recursos de shell.
- [awesome-osx-command-line](https://github.com/herrbischoff/awesome-osx-command-line): una guia més detallada per a la línia d'ordres de macOS.
- [Mode estricte](http://redsymbol.net/articles/unofficial-bash-strict-mode/) per escriure millors scripts de shell.
- [shellcheck](https://github.com/koalaman/shellcheck): una eina d'anàlisi estàtica de shell script. Essencialment, lint per a bash/sh/zsh.
- [Filenames and Pathnames in Shell](http://www.dwheeler.com/essays/filenames-in-shell.html): Lamentablement complexos detalls sobre com gestionar correctament els noms de fitxer en els scripts de shell.
- [Data Science at the Command Line](http://datascienceatthecommandline.com/#tools): més ordres i eines útils per fer ciència de dades, del llibre del mateix nom

## Exempció de responsabilitat

Amb l'excepció de les tasques molt petites, el codi s'escriu perquè altres puguin llegir-lo. Amb el poder ve la responsabilitat. El fet que *pugueu* fer alguna cosa a Bash no vol dir necessàriament que ho hàgiu de fer! ;)


## Llicència

[![Llicència de Creative Commons](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-sa/4.0/)

Aquesta obra té una llicència [Creative Commons Reconeixement-CompartirIgual 4.0 Internacional](http://creativecommons.org/licenses/by-sa/4.0/).
