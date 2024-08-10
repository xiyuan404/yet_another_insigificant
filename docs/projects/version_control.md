[code repository]()

## git "way of thinking"

startup file strcture
```bash
.git
COPYRIGHT
```


### moving data from left to right

move data from working area -> stage area
git diff
git add

move data from stage area -> repository
git diff --cached
git commit


### moving data from right to left


delete files add to stage area : `git rm --cached`

```bash
➜ cat COPYRIGHT
@copyright by xiyuan
➜ git add COPYRIGHT
➜ git status
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   COPYRIGHT
➜ git rm --cached COPYRIGHT
rm 'COPYRIGHT'
➜ git status
untracked files:
  (use "git add <file>..." to include in what will be committed)
	COPYRIGHT
```

delete files add to working are: `rm COPYRIGHT`




renaming file
```bash
➜ mv README.txt README.md
➜ git status
Untracked files:
	README.md
```




```bash
➜ git mv README.text -> README.md
Changes to be committed:
    renamed: README.txt -> README.md
```


## move branch
`git reset --soft`: don't copy
`git reset --mixed`: default, copy data from repo -> index
`git reset --hard`: copy data from repo -> index and working area

`git reset --mixed head`: unstage change in stage area
`git reset --hard head` : overwrite both working and stage with the cotent of head point commit





[^releases codeNmae]: android releases codename



??? note "android release codename"
    | Codename           | Version       | API level/NDK release |
    | :----------------- | :------------ | :-------------------- |
    | Android14          | 14            | API level 34          |
    | Android13          | 13            | API level 33          |
    | Android12L         | 12            | API level 32          |
    | Android12          | 12            | API level 31          |
    | Android11          | 11            | API level 30          |
    | Android10          | 10            | API level 29          |
    | Pie                | 9             | API level 28          |
    | Oreo               | 8.1.0         | API level 27          |
    | Oreo               | 8.0.0         | API level 26          |
    | Nougat             | 7.1           | API level 25          |
    | Nougat             | 7.0           | API level 24          |
    | Marshmallow        | 6.0           | API level 23          |
    | Lollipop           | 5.1           | API level 22          |
    | Lollipop           | 5.0           | API level 21          |
    | KitKat             | 4.4 - 4.4.4   | API level 19          |
    | Jelly Bean         | 4.3.x         | API level 18          |
    | Jelly Bean         | 4.2.x         | API level 17          |
    | Jelly Bean         | 4.1.x         | API level 16          |
    | Ice Cream Sandwich | 4.0.3 - 4.0.4 | API level 15, NDK 8   |
    | Ice Cream Sandwich | 4.0.1 - 4.0.2 | API level 14, NDK 7   |
    | Honeycomb          | 3.2.x         | API level 13          |
    | Honeycomb          | 3.1           | API level 12, NDK 6   |
    | Honeycomb          | 3.0           | API level 11          |
    | Gingerbread        | 2.3.3 - 2.3.7 | API level 10          |
    | Gingerbread        | 2.3 - 2.3.2   | API level 9, NDK 5    |
    | Froyo              | 2.2.x         | API level 8, NDK 4    |
    | Eclair             | 2.1           | API level 7, NDK 3    |
    | Eclair             | 2.0.1         | API level 6           |
    | Eclair             | 2.0           | API level 5           |
    | Donut              | 1.6           | API level 4, NDK 2    |
    | Cupcake            | 1.5           | API level 3, NDK 1    |


node pick element table theme codename

see discussion in github [Pick the next ten codenames or something like that](https://github.com/nodejs/Release/issues/291)


??? note "node list of codename for LTS releases"
    Argon (4.x 2015)
    Boron (6.x 2016)
    Carbon (8.x 2017)
    Dubnium (10.x 2018)
    Erbium (12.x 2019)
    Fermium (14.x 2020)
    Gallium (16.x 2021)
    Hydrogen (18.x 2022)
    Iron (20.x 2023)
    Jod (22.x 2024)
    Krypton (24.x 2025)
    Lithium (26.x 2026)
    Magnesium (28.x 2027)
    Neon (30.x 2028)
    Oxygen (32.x 2029)
    Platinum (34.x 2030)



another interesting codename theme

Herbs + Spices

Dill
Epazote
Fennel
Garlic
Horseradish
Italian Seasoning
Jalapeno Powder
Kaffir
Lavendar
Marjoram
Nutmeg
Oregano
Parsley
Quatre epices
Rosemary
Sage
Thyme
Urfa Biber
Vadouvan
Wasabi
Xanthan Gum
Yellow Mustard Seed
Za'atar
