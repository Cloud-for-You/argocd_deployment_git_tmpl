# {{ PROJECT }}

## Obsah dat pro nasazení

Jméno adresáře-souboru | Typ | Povinný | K čemu slouží
--- | --- | --- | ---
foo | Directory | Ne | Zdroj helmcharty, která by měla být součástí aplikace a ne deployment repozitáře. Zde je pouze jako příklad, ze kterého vzniká packages ukládaný do Nexus
application_foo.yaml | File | Ano | Nastavení deploymentu a je nejdůležitějším souborem, který uvádí jaká verze bude nasazena a kde jsou soubory, podle kterých se bude nasazovat 
helm-values | Directory | Ano | Adresář s jednotlivými values, na které se odkazuje pro nasazované helmcharty z hlavního souboru
manifests | Directory | Ano | Adresář, který obsahuje dodatečné manifesty, které nejsou zaneseny v helmchart a je potřeba je mít nasazeny v namespace

## Postup sestavení helmchart a upload do Nexus
Jak vytvořit helmchart je zdokumentováno v upstream dokumentaci helm a předpokládá se, že vývojář, který aplikaci dodává k nasazení do kontejneru tuto část ovládá. Výstupem helmcharty bude vždy sestavený tar.gz, který bude zaverzován a nahrán do Nexus. Jak vytvořit konkrétní verzi helmchartu je taktéž popsáno v upstream dokumentaci a není nutné toto duplikovat v tomto popisu.

## Upload helmchart balíčku do Nexus
```shell
$ helm repo add <PROJECT>  https://nexus_repository.example.com/repository/helm-<PROJECT>-hosted --username xxx --password xxx
```
Nainstalujeme si potřebný plugin pro práci s Nexus repository
```shell
$ helm plugin install --version master https://github.com/sonatype-nexus-community/helm-nexus-push.git
```
Nahrajeme helmchark balíček do repozitáře
```shell
$ helm nexus-push <PROJECT> step-certificates-1.28.1.tgz --username xxx --password xxx
```

