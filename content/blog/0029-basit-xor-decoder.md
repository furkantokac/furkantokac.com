---
title: "Basit XOR Decoder"
date: "2016-03-15T18:13:51+03:00"
thumbnail: "/img/0029-basit-xor-decoder.png"
categories: ["C"]
tags: ["C"]
url: "basit-xor-decoder"
---

### Açıklama :

Bu kodu yaklaşık 1 sene önce ödevim olduğu için yazmıştım bu yüzden bazı kısıtlamalara uymak zorunda kaldım. (fonksiyon vs. kullanımı yasaktı) Temiz ve açık bir şekilde yazdığım için yararı olabilir :)


### Örnek Kodlar :

Binary, secred code z : 001111000000111100001000000100010001101100010100</br>
Octal, secred code z : 074017010021033024</br>
Decimal, secred code z : 060015008017027020


### Kaynak Kod :

{{< highlight c >}}
/* Simple Decoder V1.0
 * 
 * Furkan TOKAC
 * OS Ubuntu 14.04
 * 
*/
#include <stdio.h>

int main()
{
    int choice=1, counter=0;
    char sc='A';  // sc means secretCode
    long scBin=1000001; // scBin=Binary of secret code(default A)
    
    while(choice != 3)
    {
        printf("Happy Decoder!\n");
        printf("(1) Decode text\n");
        printf("(2) Change the secret code\n");
        printf("(3) Quit\n");
        printf("You choose: ");
        scanf("%d", &choice);
        
        switch(choice)
        {
//---CASE 1------------------------------------------
            case 1:
                counter=0;
                int base=2, digit=0, match=0, decCh=0, decimal=0; // decCh=decimal of 3char
                char txCode; // Code which is entered by user
                long binCh=0, scTemp=scBin, binary=0; // binCh=binary of current character
                
                printf("You have chosen option 1\n");
                printf("Which base will you use to enter text (base 10/8/2)? ");
                scanf("%d", &base);
                if( base!=10 && base!=8 && base!=2 )
                {
                    printf("Invalid base. Try again...\n");
                    break;
                }
                while(getchar() != '\n'); // fflush has problem in linux so I created a statement to handle problem
                printf("Please enter the text to decode: ");
                scanf("%c", &txCode);
                printf("Your Decoded text is: ");
                
                switch(base)
                {
                    //---BINARY-----------------
                    case 2:
                        while(txCode != '\n')
                        {
                            counter++;
                            binary*=10;
                            if(txCode == '1') // save entered binary to "binary" variable
                                binary += 1;
                            
                            if(counter == 8) // If first 8 char digit taken, we handle it. If user enter missing char, we won't handle it
                            {
                                for(scTemp=scBin,counter=0, digit=1; counter&lt;8; counter++) // binary xor secredcode(scTemp) { if((scTemp%10) != (binary%10)) binCh += digit; digit*=10; scTemp/=10; binary/=10; } for(base=0,digit=1;binCh &gt; 0;) // XORed binary to decimal
                                {
                                    base += (binCh%10)*digit;
                                    digit*=2;
                                    binCh/=10;
                                }
                                
                                printf("%c", base);
                                binary=0;
                                counter=0;
                            }
                            scanf("%c", &txCode);
                        }
                        break;
                    
                    //---OCTAL------------------
                    case 8:
                        while(txCode != '\n')
                        {
                            counter++;
                            
                            for(match=0; (48+match) != (int)txCode; match++); // match will be int of char
                            // I don't wanna reverse and do more work to get octal. I basically did following.
                            if(counter == 1)
                                decCh = match*100;
                            else if(counter == 2)
                                decCh += match*10;
                            else if(counter == 3)
                                decCh += match;
                            
                            if(counter == 3) // If first 3 char digit taken, we handle it. If user enter missing char, we won't handle it
                            {
                                for(digit=1,decimal=0; decCh&gt;0;) // octal to decimal
                                { 
                                    decimal += (decCh%10)*digit;
                                    digit*=8;
                                    decCh/=10;
                                }
                                
                                for(digit=1,binary=0; decimal&gt;0;) // decimal to binary
                                {
                                    binary += (decimal%2)*digit;
                                    digit*=10;
                                    decimal/=2;
                                }
                                
                                for(scTemp=scBin,counter=0,digit=1; counter&lt;8; counter++) // binary xor secredcode(scTemp) { if((scTemp%10) != (binary%10)) binCh += digit; digit*=10; scTemp/=10; binary/=10; } for(base=0,digit=1;binCh &gt; 0;) // XORed binary to decimal
                                {
                                    base += (binCh%10)*digit;
                                    digit*=2;
                                    binCh/=10;
                                }
                                printf("%c", base);
                                
                                counter=0;
                                decCh=0;
                            }
                            scanf("%c", &txCode);
                        }
                        break;

                    //---DECIMAL----------------
                    case 10:
                        while(txCode != '\n')
                        {
                            counter++;
                            
                            for(match=0; (48+match) != (int)txCode; match++); // match will be int of char
                            // I don't wanna reverse and do more work to get decimal. I basically did following.
                            if(counter == 1)
                                decCh = match*100;
                            else if(counter == 2)
                                decCh += match*10;
                            else if(counter == 3)
                                decCh += match;
                            
                            if(counter == 3) // If first 3 char digit taken, we handle it. If user enter missing char, we won't handle it
                            {
                                for(digit=1,binary=0; decCh&gt;0;) // decimal to binary
                                {
                                    binary += (decCh%2)*digit;
                                    digit*=10;
                                    decCh/=2;
                                }
                                
                                for(scTemp=scBin,counter=0, digit=1; counter&lt;8; counter++) // binary xor secredcode(scTemp) { if((scTemp%10) != (binary%10)) binCh += digit; digit*=10; scTemp/=10; binary/=10; } for(base=0,digit=1;binCh &gt; 0;) // XORed binary to decimal
                                {
                                    base += (binCh%10)*digit;
                                    digit*=2;
                                    binCh/=10;
                                }
                                printf("%c", base);
                                
                                counter=0;
                                decCh=0;
                            }
                            scanf("%c", &txCode);
                        }
                        break;
                }
                printf("\n");
                break;
//---CASE 2------------------------------------------
            case 2:
                while(getchar() != '\n'); // fflush has problem in linux so I created the statement to handle problem
                printf("You have chosen option 2\n");
                printf("Which secret code will you use? ");
                scanf("%c", &sc);
                printf("Binary equivalent of the chosen code is ");
                int scDec = (int)sc, decade=1;
                
                for(counter=0,scBin=0; scDec&gt;0; counter++)
                {
                    scBin += (scDec%2)*decade;
                    decade *= 10;
                    scDec /= 2;
                }
                for(; (8-counter)&gt;0; counter++) // Write zeros if it necessary for 8bit binary
                    printf("0");
                printf("%ld\n", scBin);
                break;
//---CASE 3------------------------------------------
            case 3:
                printf("You have chosen option 3\n");
                break;
            
            default:
                printf("Invalid choice. Try again...\n");
                break;
        }
        printf("\n");
    }
    
    printf("Bye bye!\n");
    return 0;
}
{{< /highlight >}}
