# DOCTOSHOTGUN

This script lets you automatically book a vaccine slot on Doctolib in Berlin, Germany in the time period of your choice. Many thanks to @rbignon for the original idea.


<p align="center">
  <img src="https://raw.githubusercontent.com/rbignon/doctoshotgun/da5f65a1e2ecc7b543376b1549c62004a454b90d/example.png">
</p>

## Python dependencies

- [woob](https://woob.tech)
- cloudscraper
- dateutil
- termcolor
- playsound

## Run from terminal lines

For *nix systems, you can run the script via command line by following the steps below.

1. Install dependencies:

```
pip install -r requirements.txt
```

2. Run:

```
./doctoshotgun.py <city> <email> [password]
```

3. You can also apply optional arguments to customize your search as below:

```
--center "<name>" [--center <name> …] : filter centers to only choose one from the provided list
-p <index>, --patient <index>         : select patient for which book a slot
-z, --pfizer                          : looking only for a Pfizer vaccine
-m, --moderna                         : looking only for a Moderna vaccine
-d, --debug                           : display debug information
-t <days>, --time-window <days>       : set how many next days the script look for slots
--start-date <DD/MM/YYYY>             : set a specific start date on which to start looking
--dry-run                             : do not really book a slot
```

### Run with Docker

Build the image:

```
docker build . -t doctoshotgun
```

Run the container:

```
docker run doctoshotgun <city> <email> [password]
```

### Multiple cities

You can also look for slot in multiple cities at the same time. Cities must be separated by commas:

```
$ ./doctoshotgun.py <city1>,<city2>,<city3> <email> [password]
```

### Filter on centers

You can give name of centers in which you want specifictly looking for:

```
$ ./doctoshotgun.py berlin haidinhtuan@gmail.com \
      --center "Messe Berlin/ Halle 21" \
      --center "Velodrom Berlin"
```

### Select patient

For doctolib accounts with more than one patients, you can select patient just after launching the script (Don't forget to add patients to your Doctolib account first):

```
$ ./doctoshotgun.py berlin haidinhtuan@gmail.com PASSWORD
Available patients are:
* [0] Hai Dinh Tuan
* [1] Max Mustermann
For which patient do you want to book a slot?
```

You can also give the patient id as argument:

```
$ ./doctoshotgun.py berlin haidinhtuan@gmail.com PASSWORD -p 1
Starting to look for vaccine slots for Max Mustermann in 1 next day(s) starting today...
```

### Customizing your search window:

By default, the script looks for slots in the next 30 days (including the current day). However, if this is not your best option, of course you can change your search time window. For example, if you want to search in the next 5 days :

```
$ ./doctoshotgun.py berlin haidinhtuan@gmail.com -t 5
Password:
Starting to look for vaccine slots for Hai Dinh Tuan in 5 next day(s) starting today...
This may take a few minutes/hours, be patient!
```

### Look on specific date

By default, the script looks for slots in the next 30 days (including the current day). If you can't be vaccinated right now (e.g you are infected recently or simply don't belong to any priotised group) and you are looking for an appointment in a distant future, you can pass a starting date:

```
$ ./doctoshotgun.py berlin haidinhtuan@gmail.com --start-date 20/06/2021
Password:
Starting to look for vaccine slots for Hai Dinh Tuan in 7 next day(s) starting 20/06/2021...
This may take a few minutes/hours, be patient!
```

### Filter by vaccine

You can filter by the vaccine type, as explained in the table above. For example, please use option -z to specifically looking for Pfizer/BioNTech vaccine appointment.

```
$ ./doctoshotgun.py berlin haidinhtuan@gmail.com PASSWORD -z
Starting to look for vaccine slots for Hai Dinh Tuan...
Vaccines: Pfizer
This may take a few minutes/hours, be patient!
```

Similarly, Moderna vaccine appointments can be filtered with the -m option.

## Development

### Running tests

```
 $ pip install -r requirements-dev.txt
 $ pytest test_browser.py
```
