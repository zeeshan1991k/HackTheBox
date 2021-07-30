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
This pogram needs two arguments one is file itself and other is key. The program adds bytes of key together , if they does not equal `0x641` (1601 in decimal) , then it gives error `Incorrect master key`. This program also makes curl requests to `http://passmanager.htb:1234/index.php?method=select&username=administrator&table=passwords`, which indicates it is some type of password manager

