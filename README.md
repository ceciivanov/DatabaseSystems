# Υλοποίηση Συστημάτων Βάσεων Δεδομένων
# Εργασία 2

## Προσωπικά Στοιχεία

Όνομα: Tsvetomir Ivanov Α.Μ.: 1115201900066

Όνομα: Χρήστος Γαλανόπουλος Α.Μ.: 1115201900031

## Documentation

## 0. Δομη Project και οδηγίες εκτέλεσης
Τo project περιέχεται στο φάκελο code. Πέραν των αρχικών φακέλων έχει προστεθεί ένας ακόμη με όνομα files. Ο files περιέχει δύο υποφακέλους τον hash_files και τον logs, όπου στον πρώτο αποθηκεύονται τα παραγόμενα *.db αρχεία και στον logs τα αποτελέσματα εκτύπωσης του προγράμματος. Όσον αφορά την εκτέλεση του προγράμματος το Makefile περιέχει αρκετές επιλογές:

- make sht για να γίνει το compilation sht_main.c σε συνδυασμό με τα υπόλοιπα αρχεία
- make run για εκτέλεση του build/runner με τα αντίστοιχα ορίσματα
- make time για εκτέλεση με build/runner χρονομέτρηση
- make clean για διαγραφή των παραγόμενων αρχείων
 
Στον φάκελο include, έχουν προστεθεί:

- ενα αρχειο επικεφαλίδας data.h το οποίο περιέχει πίνακες με names, surnames και cities.
- ενα αρχειο επικεφαλίδας ht.h το οποίο περιέχει τις κοινές δομές για το hash_table και secondary_hash_table.


## 1. Περιγραφή Main συνάρτησης και αποτελέσματα προγράμματος
Η main συνάρτηση του προγράμματος περιέχεται στο αρχείο sht_main.c στον φάκελο examples. H main έχει την εξής ροή:

- άνοιγμα αρχείου στο οποίο θα γραφτούν τα αποτελέσματα.
- δημιουργία πρωτεύοντων μαζί με δευτερεύοντα αρχεία και άνοιγμα αυτών.
- εισαγωγή εγγραφών σε αυτά.
- εκτύπωση 1) ολων των εγγραφών για το πρωτεύον και δευτερεύον 2) εκτύπωση τυχαίας εγγραφής για το πρωτεύον με βάση καποιο id, για το δευτερεύον με κάποιο index_key.
- εκτύπωση εγγραφών απο τα δύο δευτερεύοντα ευρετήρια με βάση καποιο τυχαίο index_key.
- εκτύπωση αποτελεσμάτων ζεύξης (Inner Join) για τα αρχεία στο απο πάνω πεδίο index_key.
- εκτύπωση αποτελεσμάτων ζεύξης στην περίπτωση που το index_key είναι NULL.
- εκτύπωση στατιστικών για πρωτεύον και δευτερεύον.

Ορίζονται επίσης οι σταθερές
- NO_RECORDS -> πλήθος εγγραφών που πρόκειται να εισαχθούν στα πρωτεύοντα αρχεία.
- GLOBAL_DEPTH -> αρχικό βάθος με το οποίο δημιουργούνται τα αρχεία
- INDEX_KEY -> 'city' or 'surname' , το πεδίο κλειδί στο οποίο θα δημιουργηθούν τα δευτερεύοντα αρχεία
- NO_NAMES -> πλήθος ονομάτων για εισαγωγή εγγραφών (μέγιστο μέγεθος = 500)
- NO_SURNAMES -> πλήθος επιθέτων για εισαγωγή εγγραφών (μέγιστο μέγεθος = 500)
- NO_CITIES -> πλήθος πόλεων για εισαγωγή εγγραφών (μέγιστο μέγεθος = 300)

Οι τελευταίες τρεις σταθερές καθορίζουν το πληθος των στοιχείων που θα ληφθούν υπόψιν κατα την εισαγωγή εγγραφών μέσω της rand(). Απο αυτές εξαρτάται και το πλήθος των εγγραφών που θα πετύχει το πρόγραμμα να εισάγει στο δευτερεύον ευρετήριο (όπως αναλύεται παρακάτω). Επίσης για μικρες σταθερές φαίνεται καλύτερα το αποτέλεσμα της InnerJoin καθώς υπάρχουν λιγότερα στοιχεία απο τα οποία θα επιλεχθεί καποιο ως index_key πανω στο οποίο θα γίνει η ζεύξη.

### Αφήνεται στον χρήστη να αλλάξει τις σταθερές καθορίζοντας έτσι το πόσες εγγραφές θέλει να εισαχθούν στα αρχεία, με βάση ποιο κλειδί να δημιουργηθεί το δευτερεύον, με τι βάθος να ξεκινήσουν τα ευρετήρια και αντίστοιχα το πλήθος των στοιχείων απο τα οποία θα δημιουργούνται οι εγγραφές.

### Περιγραφή Αποτελεσμάτων
Για block_size = 512 bytes, ενα data block χωράει 512 - 4/24 = 21 εγγραφές (24 ειναι το μέγεθος της κάθε εγγραφής).
Αν ο αριθμός των πόλεων ή αντίστοιχα των επιθέτων (ανάλογα ποιο ειναι το index_key) είναι περιορισμένος, ενώ ο αριθμός των εγγραφών αυξάνεται σημαντικά, απο ενα σημείο και μετά το δευτερεύον ευρετήριο θα φτάνει στο μέγιστο βάθος επειδή κάθε πόλη θα προορίζεται στον ίδιο κάδο (πολλαπλές εγγραφές με ίδιο index_key) με αποτέλεσμα αυτός να γεμίζει. Εφόσον δεν υλοποιούνται κάδοι υπερχείλισης, η περίπτωση αυτή ειναι οριακή και ειναι αναμενόμενο να μην μπορούν να εισαχθούν όλες οι εγγραφές. Αν οι σταθερές NO_CITIES, NO_SURNAMES μειωθούν παρατηρέιται το παραπάνω φαινόμενο για μεγάλο αριθμό εγγραφών. Αν οι τιμες τους είναι οι μέγιστες (300 και 500), μπορούν να εισαχθούν εως και 1000 εγγραφές.

#### Παραδοχές
- Κάθε bucket δείχνει σε ένα data_block
- Μέγιστος αριθμός depth=13 για block_size=512. Επεξήγηση: ένα metadata block μπορεί να αποθηκεύσει τα ids από (512 - 18)/4=123 hash blocks και για depth=14 έχουμε (2^14)/128=128 hash blocks όπου το πρώτο 128 είναι ο αριθμός bucket χωράει ένα hash block
 Άρα για block_size=512 ισχύει 0 < depth < 14.


## 2. Σχεδιαστικές επιλογές και παραδοχές

Ένα αρχείο δευτερεύοντος ευρετηρίου επεκτατού κατακερματισμού (SHT) περιέχει:

- 1 metadata block
- 1 ή παραπάνω hash blocks
- 2 ή παραπάνω data blocks

### a) Δομή metadata block

Ένα metadata block έχει την ακόλουθει δομή: 

- αναγνωρισιτκό του hash file -> 'HashFile'
- χαρακτήρας που καθορίζει σε ποιο πεδίο κλειδί δημιουργείται το δευτερεύον -> 'c' για city, 's' για surname
- τρέχον global depth
- αριθμός hash blocks
- id για το 1ο hash block
- id για το 2ο hash block
- ...

### b) Δομή hash block

Ένα hash block έχει την ακόλουθει δομή:

- bucket 0 που περιέχει ένα data block id
- bucket 1 που περιέχει ένα data block id
- ...

### c) Δομή data block

Ενα data block έχει την ακόλουθη δομή:

- τρέχον local depth
- αριθμός εγγραφών
- 1η εγγραφή
- 2η εγγραφή
- ...

### Secondary Hash Table
Οπως και με το πρωτεύον ευρετήριο, το hash table είναι νοητό, εννοώντας ότι δεν υπάρχει κάποιο struct για την αναπαράστασή του αλλά συνίσταται από όλα τα hash blocks που περιέχουν τα buckets.

### Hash_Function
Το hash function δέχεται ως όρισμα το index-key μιας εγγραφής και κάνει καποιες κατάλληλες για συμοβολοσειρές ενέργειες κατακερματισμού επιστρέφοντας ενα string απο 14 χαρακτήρες ('0' και '1').
  
### HF_Info open_files: Πίνακας ανοικτών αρχείων
Χρησιμοποιείται ένας πίνακας χωρητικότητας 20, για αποθήκευση οποιονδήποτε ανοιχτών αρχείων, είτε ειναι πρωτευοντα ευρετήρια είτε δευτερεύοντα. Εχουν προστεθεί κάποια extra πεδία στο struct HF_Info:

- file descriptor
- τρέχον βάθος
- αριθμός εισαχθέντων εγγραφών
- τρέχον αριθμός κάδων
- τρέχον αριθμός hash blocks
- ονομα του αρχείου
- τύπος ευρετηρίου (πρωτεύον ή δευτερεύον)
- θεση στον πίνακα των ανοικτών αρχείων του αντίστοιχου αρχειου πρωτεύοντος ευρετηρίου πάνω στο οποίο εχει δημιουργηθεί ενα δευτερεύον
- τυπος κλειδιού (index-key) στην περίπτωση που ειναι δευτερεύον -> ('c' για city, 's' για surname)
- split -> χρησιμοποιείται για τον χειρισμό των split που μπορεί να προκύψουν σε ενα πρωτεύον ευρετήριο επηρεάζοντας ανάλογα το δευτερεύον

## 3. Ανάλυση λειτουργικότητας HT, SHT συναρτήσεων
 
### HT_Init, SHT_Init:
Επειδή η HT_Init αρχικοποιεί τον αδειο πίνακα open_files, η SHT_Init δεν χρειάζεται να κάνει καποια λειτουρία.

###  HT_InsertEntry:

#### Τροποποίηση πρότυπου συνάρτησης:
Το πρότυπο της συνάρτησης εισαγωγής εγγραφής στο πρωτεύον ευρετήριο εχει τροποποιηθεί ως εξης:
- αλλαγή του δεικτη σε δομη UpdateRecordArray σε διπλό, ώστε να ειναι δυνατή η τροποποιηση του πίνακα των δομων αυτών απο την συνάρτηση και η επιστροφή του στην συνάρτηση που την καλεί.
- προσθήκη επιπλέον ορίσματος, δείκτη σε ακεραιο που δηλώνει το μέγεθος του πίνακα updateArray, καθώς αυτος δημιουργείται δυναμικά με malloc στην HT_InsertEntry και πρέπει να είναι γνωστός.

#### Τροποποίηση λειτουργίας:
Η HT_InsertEntry κάνει τις κατάλληλες ενέργειες ώστε να επιστρέψει την θέση στην οποία εισήχθη μια νέα εγγραφή, και να φτιάξει σε περίπτωση split τον πίνακα updateArray ο οποίος αποθηκεύει τις εγγραφές που άλλαξαν μετά την διάσπαση των bucket.

### SHT_CreateSecondaryIndex:
Δημιουργεί ένα νέο αρχείο επεκτατού κατακερματισμού δευτερεύοντος ευρετηρίου με δοσμένο βάθος, πεδίο κλειδί πάνω στο οποίο θα γίνει η ευρετηρίαση, και όνομα του αντίστοιχου αρχείου πρωτεύοντος ευρετηρίου. Η υλοποίηση της είναι παρόμοια με αυτήν της κατασκευής του πρωτεύοντος,  με την μόνη διαφορά την προσθήκη της πληροφορίας για το πεδίο κλειδι στο metadata του αρχείου.

### SHT_OpenSecondaryIndex:
Ελέγχεται ότι το αρχείο προς άνοιγμα είναι ένα valid hash file με ορθό index-key, ενημερώνεται κατάλληλα ο πίνακας open_files ώστε να περιέχει τις πληροφορίες που χρειάζονται και επιστρέφεται η θέση του πίνακα στην οποία αποθηκεύτηκαν.

#### Σχόλιο: Επειδή ο πίνακας των ανοικτών αρχείων είναι global και επομένως γνωστός και στην main συνάρτηση, επιλέγεται η τελευταία να ενημερώσει όταν φτιαχτεί και ανοιχτεί το δευτερεύον, το πεδίο που δηλώνει την θέση του αντιστοιχου πρωτεύοντος αρχείου. Η παραδοχή αυτή γίνεται για να μην επιβαρύνεται με παραπάνω πληροφορίες το metadata block του δευτερεύοντος.

### SHT_CloseSecondaryIndex:
Κλείνει το ανοιχτό δευτερεύον hash file και ενημερώνεται με κατάλληλο τρόπο ο πίνακας open_files.

### SHT_SecondaryUpdateEntry:

#### Τροποποίηση πρότυπου συνάρτησης:
Προστίθεται ως όρισμα, ένας ακέραιος για το μέγεθος του πίνακα.

#### Λειτουργία:
Για ολες τις εγγραφές που έχουν αποθηκευτεί στον πίνακα updateArray, εκτελεί σειριακή αναζήτηση στο αρχείο του δευτερεύοντος ευρετηρίου, και αφου τις βρει ενημερώνει κατάλληλα τις αλλαγές που έγιναν.

### SHT_InnerJoin:
Η διαδικασία της ζεύξης υλοποιείται ως εξής:
Αν το πεδιο index_key δεν ειναι NULL αλλά κάποια πόλη ή επίθετο (π.χ Athens ή Ioannidis) τότε εκτελείται σειριακή αναζήτηση στο δευτερεύον του πρώτου αρχείου (sindexDesx1) και αφου βρεθεί η εγγραφή, αναζητούνται εγγραφές με το ιδιο πεδίο στο δευτερευον του δεύτερου αρχείου. Στην περίπτωση που υπάρχουν πανω απο μια εγγραφες στο πρώτο αρχείο θα εκτυπωθουν σε διαφορετικες γραμμες όλοι οι δυνατοί συνδυασμοι όπως στην παρακάτω εικόνα.
<img width="832" alt="Screenshot 2022-01-08 at 18 12 59" src="https://user-images.githubusercontent.com/61785341/148686500-69ee0f24-a4cb-4766-a819-f12095717eca.png">



Αν το πεδιο index_key είναι NULL, εκτελείται σειριακά η ιδια διαδικασία με την διαφορά οτι ως index_key λαμβάνεται καθε φορά το index_key απο τo κάθε record του δευτερεύοντος ευρετηρίου για το πρώτο αρχείο. Ετσι εκτυπώνονται σε διαφορετικες γραμμές όλες οι δυνατές ζεύξεις σε καποιο πεδίο για τα δύο αρχεία όπως στην παρακάτω εικόνα. 
<img width="718" alt="Screenshot 2022-01-09 at 16 26 13" src="https://user-images.githubusercontent.com/61785341/148686566-ca65f50d-3ee8-420b-a60e-76c4a2f928f3.png">
