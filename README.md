# Folder system [v0.1.0.0] (2019-02-13)

## Intro
This project was created to provide advises on how to establish an efficient, reliable and scalable folder structure, whether for yourself or for an organisation. These advices are to be taken more like the result of a thorough analysis than the work of professionals in the matter. Although it was mainly designed for computer use, many of it's elements do apply for real physical applications. It aims at easing folder management and tries to be as agnostic as possible.

>In this text, **( )** is used for info, **[ ]** for a parameter and **{ }** is optional;

## Folders and files management
Having a structure is essential to deploy, organise and retrieve efficiently what you have to. It can be divided in the various following chapters :

### Concepts
For a better understanding of this text, a few concepts are important to grasp :
* Publishing a document happens the moment you provide it to a tier.
* A *document class* is a type of document (ex.: an invoice);
* A *document* is an element which has versions saved under *files*. (ex.: *invoice-2857251* is the document while *20190212-211500_invoice-2857251_v1.3.4.odt* is the file);
* A document can be composed of many files (ex.: an invoice may have a work-hours list);
* Some folders are documents (see [Bundleing](#bundleing));
* The *tree* is the list of folders, from top (*root*) to bottom (*leafs*).

#### Organization
There are two main folder structures :

||Grouped|Associated|
|:--|:-:|:-:|
|**Description**|When all documents of a same type are placed in the same folder with no regard to the relations it may have. This suits well the case of an organisation's invoices for instance where they would all be classified under the *invoices* folder.|When a file is placed in the folder of it's most relevant relation, most often the owner or the reference, with no regard to it's type. This case mostly apply for shared structures or when a document has enough importance in the logic to be holder of his relations.
|**Pros**|Logic kept together, easier to browse through same type files|More human minded, easier to adjust to physical structures|
|**Cons**|May end up with folders having numerous files|Files are scattered so they are harder to batch-operate on, especially physically|

**Visual representations :**

*Grouped*
* root
    * administrative
        * invoices
            * invoice-1
            * invoice-2
            * invoice-3
    * sales
        * orders
            * order-1
            * order-2
            * order-3
     * clients
         * 001
         * 002
         * 003
     * providers
         * AAA
         * BBB
         * CCC

*Associated*
* root
    * clients
        * 001
            * invoices
                * invoice-3
            * orders
                * order-2
        * 002
            * invoices
                * invoice-1
                * invoice-2
            * orders
                * order-1
                * order-3
        * 003
    * providers
        * AAA
        * BBB
        * CCC

>As you can see, *grouped* structure has less depth, but significantly more distribution imbalance.

A usual folder *tree* is composed of both as different document classes have different handling requirements. The right structure has to be determined for each class and may sometimes even be a combination.

### Naming
File naming is critical, as much for the users as for the hardwares. You will have to find a file back every so often and you have to make sure it will be hassle free. You want to be able to use key tags such as *when*, *what* or *who*. You also want it to be meaningful yet quickly readable. At the same time, hardware is also involved in the operation and it's limitations impose restrictions. You don’t want to lose any file because the name you saved it on worked well on your computer, but your friend tried with the one of his own and crashed the whole thing. Thus, here are a few guidelines to follow to ensure cross-user and cross-platform stability and operability :
##### Restrictions
* No space (use -, + or _ instead)
    * Use - to separate words
    * Use _ to separate parts
* No slash or path characters ( : / \\ )
* No special characters (±@£¢¤¬¦¼"/$%&*[\](){}<>|)
* No punctuation (,.;`^¸!?)
* No link / symlink (they are too much locked to their environment and renaming of the source file doesn't update them)
* Use capital letters only to contrast or when necessary (names, acronyms)
* Use leading zeros [000-009] (it's length has to be set from the start according to the document class' needs for incrementation but 2-3 should fit most)

#### Timestamp
Hardware time stamping is not reliable. For instance, if you *copy* a file, it's *created at* timestamp will be set to the one of the operation. As this is an essential information, you must include it in the file's name to keep it. To ensure optimal ordering, the *year-month-day hour-minute-second timezone* format is used. The *minute* part may be left out for files that are more likely to be released on a daily basis rather than on timely one. The *timezone* part is the uppercased [abbreviated name](https://en.wikipedia.org/wiki/List_of_time_zone_abbreviations) of the one in which the *timestamp* is. It is optional unless the context requires otherwise.

**Parts and format** : `[YYYYMMDD]{-HHMMSS}{-ZZZ}`

**Example** : `20190212-184800-EST`

#### Type
A *type* is a tag for similar recurring files. It's goal is to partly overcome limitations of the *associated folder* structure and be able to find them easily across the *tree*. Try to keep it within **3 characters** of length with an optional *4th modificator*, which still offers you a whopping *17 576* available combinations. This part is optional but recommended and can be :
* *pv[\*]* : **Minutes** (written summary of a talk)
    * `pvc{}` : By phone (optional parameter is destination : **f**rom / **t**o)
    * `pvp` : In person
    * `pvm` : Vocal message
    * `pvw` : Workshop
    * `pvg` : Council meeting (booked for public bodies)
* *rec[\*]* : **Recordings**
    * `reca` : Audio recording (most often of a talk)
    * `recv` : Video recording
* **Texts**
	* `inv` : Invoice
	* `rep` : Report
* **Medias**
    * `img` : Image
    * `vid` : Video
    * `pln` : Plan

#### Description
For a file description to be efficient, it has to be short yet expressive. This means it has to summarize the most of the content in a reasonable length of **50 characters**. Only use nouns, verbs and ultimately adjectives as theses are enough to convey a complete message. The best description will always be a unique identifier to the document for the type such as a **reference number**, a **project identifier** or an **ID**. When these do not apply, a *name*, a *place* (*address*) or a *reason* are good second choices.

>*sources* :
>* https://www2.staffingindustry.com/row/Editorial/Archived-Blog-Posts/Adam-Pode-s-Blog/Probably-the-best-file-naming-convention-ever

#### Complete format
**Parts** : `[timestamp]{_type}_[description]{_version}.<fileExtension>`

**Regex** : `((\d{8})(-(\d{6}))?(-([A-Z]{2,5}))?)(_([a-zA-Z0-9]{2,5}))?_([a-zA-Z0-9\-]{1,50})((_(v\d*_\d*_\d*))?(_(d(raft)?\d*))?)??\.([a-z0-9]{2,10})$` (*only supports number markers versions so far*)

**Format** : `[YYYYMMDD{-HHMMSS}{-ZZZ}]{_*****}_[**********]{_v*_*_*{_d*}}.***`

**Example** : `20190212-184800-EST_pvpf_Eric-Bernard_v1_4_3_d2.odt`

>In the regex, the parts are respectively group [1: (2, {4} & {6})], {8}, [9], {10: (12 & 14)}. The file extension is 16.
>
>*source* : https://www.regular-expressions.info

#### Identifier generation
When creating a new document class, you have to think how you are going to generate it's of it documents a unique identifier. This generation should take into account key identifying elements and exclude any of those previously provided in the file name. Most often it's only going to be the name of the document suffixed of an incremented number. In more complex situations, you may have to establish an identifying convention. That convention should be provided in the document's class container folder. 

### Versioning
Upon the life cycle of a document, it will likely be modified and thus, many versions of it may be produced. Keeping a history of them offers you better tracking and safety.
In order to do so, a new file must be created everytime a document is published.
Versions are represented by a string and/or *3* number markers separated by a dot (.) : *major*, *minor* and *fix* in this order. Each of them is associated to a scope of change that happens to the document. Sometimes when context allows, *fix* and *minor* markers can be merged into a single one. String markers are to be used in specific case like distribution (private, public, media, draft) or variations (branches).

>This method, although a bit simplified, is standardized and is the same as the one used for this project.

Before it's release, a file is a draft. Drafts must be suffixed by *_draft* or *_d*. Optionally, you can also version them, but with simpler logic (single number).
To ensure readability, always prefix the version part of a *v*. The version of the first official publication must be *v1.0.0*. If you plan on providing a document previous to it's official publication, keep *0* for *major* and only change *minor* and *fix* and use *string* marker if required. Everytime you update a marker, all those lower must be reset to *0*.

**Format** :

string : `v*`

numbers : `v*_*_*`

both : `v*-*_*_*` or `v*_*_*-*`

draft : `_draft*` or `_d*`

**Examples**

string : `vPublic` (note the capitalized first letter of the string)

numbers : `v1_5_12`

both : `vPublic-0_14_3` or `v0_14_3-public` (completely different meanings!)

draft : `_draft` or `_d4`
 > Here you can see that the dots in the version were
 changed for underscores (_) in
 respect to this project's naming restrictions.

```
Table of the markers and their associated changes
```

|Marker|major|minor|fix|
|:--|:-:|:-:|:-:|
|**Change in**|meaning|content|display|
|**Change examples**|publication, mandate, terms, output, ...|name, address, ...|mistake, position, ...|

>This is a quick list and every release must be evaluated on a per changes basis.

Here are some examples of file names with version :

`20190212-210000_invoice-2857251_v1_1_5.odt`
`20190212-184800_invoice-2857251_v1_1_6_draft.odt`
`20190212-184800_invoice-2857251_v1_1_6_d2.odt`
`20190212-184800_invoice-2857251_v1_1_6.odt`
`20190212-184800_invoice-2857251_vPublic.odt`
>Here you get that the third draft of the *v1.1.6* was the one released.

>Versioning does not apply to static files such as pictures and minute recordings as these are fixed in their nature.

> *sources* :
>* https://medium.com/sapioit/why-having-3-numbers-in-the-version-name-is-bad-92fc1f6bc73c

### Templating
Often you will find yourself with recurring folder structures. For instance, if you have an invoices folder in which you regularly create sub-folders to organize them, you can fasten the process by making a template folder which will contain all the default files and folder. Document classes are a good thing to template.

### Bundleing
When a document is composed of many files or has more than one version, these must be grouped inside a folder named after the document. For uniformity reason, when this happens, files contained shall only be name with *timestamp* and *version* parts.

### Explaining
Whenever you make a template, have a particular folder structure or simply want to provide a user with *instructions*, use a `readme.md` file to explain it.

### Backuping
It is a significant and required measure for you to duplicate and backup your informations. Data loss is something you want to avoid no matter what, may it be from accidental deletion, corruption, a virus or lately ransomwares. For this, I recommend [FreeFileSync](https://freefilesync.org) (**FFS**) which is free, open source and has great functionalities.

### Sharing
You can use your operating system to share any file to users on your local network (password protection). This feature can be used to centralize common informations and unify structure through users. In a business context, this structure may be on a central server as which can also host isolated or private directories.
Sometime it happens you have a folder structure in which you have public (or common) folders in which you would like to allow users to have private one. To achieve this without duplicating it, you simply have to mirror it somewhere else. Once again I suggest using the software (**FFS**) presented in [Backuping](#backuping).

Let's says `A` has the following public *tree* :
* Main folder
    * Folder A
        * Subfolder 1
    * Folder B
    * Folder C

Now, if `B` wants to have the same structure, he just has to mirror it. Any change `B` may want in the *tree*, unless being in a private folder, must be applied to the public *tree* and synced back. As **FFS** wont sync empty folders, they can be by placing an empty tag like an *.endpoint* file in it so that it gets tracked. You should not clone but only mirror the structure of the public *tree* folder to avoid file duplication.

### Clouding
The use of a *cloud* is a good idea, either with a web service like the free and open-source project [Nextcloud](https://nextcloud.com/) or with an *FTP*. Not only does it allow you to access your data anywhere but, depending on your provider, will also greatly extend your sharing, auditing and logging options. Most modern services offer public and per-user/per-file basis sharing options. This way, you can distribute your file to friends or teammate outside of your working network.
The *cloud* can be used as your file source (repository), but it is better for you only to make it mirror your local structure as remote working has it's downsides. To do so, you can use free softwares such as [RaiDrive](https://www.raidrive.com/) or [CloudBuckIt](https://www.cloudbuckit.com/) to mount locally an external folder over various protocols (*FTP*, *WebDAV*, *branded providers*).

### Updating, transforming and renaming
As this system will evolve so will it's naming and structure conventions and that may require you to apply modifications to your *tree* configuration. This may first scare you but was taken into account while developing this project. There are a bunch of available softwares allowing to execute batch rename/move actions on files. Such services and the use of standardized methods and formats in the present system allows those applying it's guidelines to expect fluent updates.

>*More sources* :
>* https://support.apple.com/fr-ca/HT202808
>* https://www.gla.ac.uk/media/media_359400_en.pdf
>* https://www2.le.ac.uk/services/research-data/organise-data/naming-files
>* https://library.stanford.edu/research/data-management-services/data-best-practices/best-practices-file-naming
>* https://regex101.com

```
2019 © - Louis Pascal Laforest
```
