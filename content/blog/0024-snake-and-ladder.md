---
title: "Snake and Ladder"
date: "2015-06-30T01:05:12+03:00"
thumbnail: "/img/0024-snake-and-ladder.jpg"
categories: ["C"]
tags: ["C"]
url: "snake-and-ladder"
---

### Açıklama :

Küçüklüğümüzde oynadığımız yılan ve merdiven oyununu ödev olarak C'de yazmıştım. Şuan biraz karışık gözükebilir zaman sınırlı diye hızlı yazmıştım bazı yerleri. Yakın zamanda koda yorumlar eklerim. Zamanla, özellikle talep çok olursa kodu gözden geçiririm.


### Kaynak Kod :

{{< highlight c >}}
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define LADDER 500
#define LSTART 501
#define LFINIS 502

#define PLAYER 700
#define COMPUTER 701

#define SNAKE1 600
#define HEAD1 601
#define TAIL1 602

#define SNAKE2 610
#define HEAD2 611
#define TAIL2 612

#define SNAKE3 620
#define HEAD3 621
#define TAIL3 622

int** create2Darray(int xAxis, int yAxis);
void locateStraight(int **a, int aoi, int length, int x, int y, int character); // I can make "size" instead of x and y variable but I want more flexible program
void drawStraight(int **a, int length, int locx, int locy, int character);
int controlStraight(int **a, int length, int locx, int locy);
int controlCurlySnake(int **a, int locx, int locy);
void drawCurlySnake(int **a, int locx, int locy, int snake);

void initializeBoard(int **a, int x, int y);
void initializeSnakesLadders(int **a, int size);
void move(int **a, int x, int y, int *locx, int *locy, int *direction, int character, int *temp, int temp27);
void step_size(int **a);
void locateLadder(int **a, int aoi, int length, int x, int y); // I can make "size" instead of x and y variable but I want more flexible program
void locateCurlySnake(int **a, int nocs, int x, int y);
void locateStraightSnake(int **a, int nos, int length, int x, int y);
void checkSnakes(int **a, int *locx, int *locy, int x, int y);
void checkLadders(int **a, int *locx, int *locy, int x, int y);
void displayBoard(int **a, int x, int y);
void displayResults(int winner, int rounds);

int main()
{
    int **board, selection=0, size=0, playerx=0, playery=0, pdirection=1, ptemp=1, compx=0, compy=0, cdirection=1, ctemp=1, rounds=0, turn=0, finy=0, finx=0;
    char choice='Y';
    srand(time(NULL));
    printf("For 10x10 board, enter 1\nFor 15x15 board, enter 2\n");
    scanf("%d", &selection);
    while( selection<1 || selection>2 )
    {
        printf("Please enter 1 or 2: ");
        scanf("%d", &selection);
    }
    while(getchar() != '\n'); //FFLUSH has problem in linux so I use this statement.
    if( selection==1 )
        size=10;
    else
        size=15;
    
    playery=size-1;
    compy=size-1;
    
    board = create2Darray(size, size);
    initializeBoard(board, size, size);
    initializeSnakesLadders(board, size);
    
    if( size%2!=0 )
        finx=size-1;
    
    rounds=1+rand()%6;
    selection=1+rand()%6;
    printf("Dices rolled to choose first player.\n");
    
    while( rounds==selection )
    {
        rounds=1+rand()%6;
        selection=1+rand()%6;
    }
    printf("Player's dice : %d --- Computer's dice : %d\n", rounds, selection);
    
    if( rounds>selection )
    {
        turn = PLAYER;
        printf("Player will play first.\n");
    }
    else
    {
        turn = COMPUTER;
        printf("Computer will play first.\n");
    }
    printf("\n\n");
    
    rounds=0;
    choice='Y';
    while( (choice=='Y' || choice=='y') && board[finy][finx]!=PLAYER && board[finy][finx]!=COMPUTER )
    {
        rounds++;
        if( turn==PLAYER )
        {
            printf("TURN : PLAYER\n");
            move(board, size, size, &playerx, &playery, &pdirection, PLAYER, &ptemp, ctemp);
            turn=COMPUTER;
        }
        else
        {
            printf("TURN : COMPUTER\n");
            move(board, size, size, &compx, &compy, &cdirection, COMPUTER, &ctemp, ptemp);
            turn=PLAYER;
        }
        displayBoard(board, size, size);
        printf("Do you want to continue to play (Y/y: Yes, N/n: No):");
        scanf("%c", &choice);
        while(getchar() != '\n'); //FFLUSH has problem in linux so I use this statement.
        printf("\n\n");
    }
    
    if( board[finy][finx]==PLAYER )
        displayResults(PLAYER, rounds);
    else
        displayResults(COMPUTER, rounds);
    
    return 0;
}

void initializeSnakesLadders(int **a, int size)
{
    if( size==10 )
    {
        locateLadder(a, 2, 3, size, size);
        locateLadder(a, 1, 5, size, size);
        locateCurlySnake(a, 2, size, size);
    }
    else if( size==15 )
    {
        locateLadder(a, 2, 3, size, size);
        locateLadder(a, 1, 5, size, size);
        locateLadder(a, 1, 6, size, size);
        locateStraightSnake(a, 1, 9, size, size);
        locateCurlySnake(a, 2, size, size);
    }
}

void checkSnakes(int **a, int *locx, int *locy, int x, int y)
{
    int i=0, j=0;
    
    if( a[*locy][*locx]==HEAD1 )
    {
        for(i=0; i<y; i++)
            for(j=0; j<x; j++)
                if( a[i][j]==TAIL1 )
                {
                    *locy=i;
                    *locx=j;
                }
    }
    else if( a[*locy][*locx]==HEAD2 )
    {
        for(i=0; i<y; i++)
            for(j=0; j<x; j++)
                if( a[i][j]==TAIL2 )
                {
                    *locy=i;
                    *locx=j;
                }
    }
    else if( a[*locy][*locx]==HEAD3 )
    {
        for(i=0; i<y; i++)
            for(j=0; j<x; j++)
                if( a[i][j]==TAIL3 )
                {
                    *locy=i;
                    *locx=j;
                }
    }
}

void checkLadders(int **a, int *locx, int *locy, int x, int y)
{
    int i=0;
    
    if( a[*locy][*locx]==LFINIS )
    {
        for(i=1; i>0; )
        {
            if( a[*locy][*locx]==LSTART )
                break;
            else
                (*locy)--;
        }
    }
}

void move(int **a, int x, int y, int *locx, int *locy, int *direction, int character, int *temp, int temp2)
{
    int dice1=0, dice2=0, left=0;
    
    a[*locy][*locx]=*temp;
    
    dice1 = 1+rand()%6;
    dice2 = 1+rand()%6;
    
    if( dice1==dice2 )
    {
        if( *direction==1 )
        {
            left = *locx-dice1;
            if( left<0 )
            {
                if( *locy+1>=y )
                {
                    *locx=0;
                    *locy=y-1;
                    *direction=1;
                }
                else
                {
                    (*locy)+=1;
                    *direction=2;
                    (*locx)+=left;
                }
            }
            else
                (*locx)-=dice1;
        }
        else
        {
            left = *locx+dice1;
            if( left>=x )
            {
                if( *locy+1>=y )
                {
                    *locx=0;
                    *locy=y-1;
                    *direction=1;
                }
                else
                {
                    (*locy)+=1;
                    *direction=1;
                    (*locx)=(x-1)+x-(*locx+dice1);
                }
            }
        }
    }
    else if( (dice1==5 && dice2==6) || (dice1==6 && dice2==5) )
    {
        int counter=0;
        while(counter < 14)
        {
            if( *direction==1 )
            {
                (*locx)++;
                if( *locx>=x )
                {
                    *locx=x-1;
                    if( (*locy-1)<0 )
                    {
                        if( y%2==0 )
                        {
                            *locx=0;
                            *locy=0;
                        }
                        else
                        {
                            *locx=x-1;
                            *locy=0;
                        }
                        break;
                    }
                    else
                    {
                        (*locy)--;
                        *direction=2;
                    }
                }
            }
            else
            {
                (*locx)--;
                if( *locx<0 )
                {
                    *locx=0;
                    if( (*locy-1)<0 )
                    {
                        if( y%2==0 )
                        {
                            *locx=0;
                            *locy=0;
                        }
                        else
                        {
                            *locx=x-1;
                            *locy=0;
                        }
                        break;
                    }
                    else
                    {
                        (*locy)--;
                        *direction=1;
                    }
                }
            }
            counter++;
        }
        
    }
    else
    {
        if( *direction==1 )
        {
            left = *locx+dice1+dice2;
            if( left>=x )
            {
                if( *locy-1<0 )
                {
                    *locx=0;
                    *locy=0;
                }
                else
                {
                    (*locy)-=1;
                    *direction=2;
                    (*locx)=(x-1)+x-(*locx+dice1+dice2);
                }
            }
            else
                (*locx)+=dice1+dice2;
        }
        else
        {
            left = *locx-(dice1+dice2);
            if( left<0 )
            {
                if( *locy-1<0 )
                {
                    *locx=0;
                    *locy=0;
                }
                else
                {
                    (*locy)-=1;
                    *direction=1;
                    (*locx)=left*(-1)-1;
                }
            }
            else
                (*locx)-=(dice1+dice2);
        }
    }
    printf("Dice1 : %d --- Dice2 : %d\n", dice1, dice2);
    
    if( a[*locy][*locx]==PLAYER || a[*locy][*locx]==COMPUTER )
        *temp = temp2;
    else
        *temp = a[*locy][*locx];
    checkSnakes(a, locx, locy, x, y);
    checkLadders(a, locx, locy, x, y);
    a[*locy][*locx]=character;
}

//nocs:numberOfCurlySnake --- x:xSizeOfBoard
void locateCurlySnake(int **a, int nocs, int x, int y)
{
    int locx=0, locy=0, tempx=0, tempy=0, keyx=0, keyy=0, snake=SNAKE1;
    
    for(; nocs>0; nocs--)
    {
        locx=rand()%(x-4);
        tempx=locx;
        locy=rand()%(y-2);
        tempy=locy;
        
        while( controlCurlySnake(a, tempx, tempy)!=1 )
        {
            if( keyx==0 )
            {
                tempx++;
                if( tempx>(x-5) )
                {
                    keyx=1;
                    tempx=locx;
                }
            }
            if( keyx==1 )
            {
                tempx--;
                if( tempx<0 )
                {
                    keyx=0;
                    tempx=locx;
                    if( keyy==0 )
                    {
                        tempy--;
                        if( tempy<0 )
                        {
                            keyy=1;
                            tempy=locy;
                        }
                    }
                    if( keyy==1 )
                    {
                        tempy++;
                        if( tempy>y-3 )
                        {
                            printf("Exception happened when map creating.\n");
                            exit(0);
                        }
                    }
                }
            }
        }
        drawCurlySnake(a, tempx, tempy, snake);
        snake+=10;
    }
}

int controlCurlySnake(int **a, int locx, int locy)
{
    int i=0;
    
    for(i=0; i<3; i++, locx++)
        if( a[locy][locx]>300 )
            return 0;
    locx--;
    for(i=0; i<3; i++, locy++)
        if( a[locy][locx]>300 )
            return 0;
    locy--;
    for(i=0; i<3; i++, locx++)
        if( a[locy][locx]>300 )
            return 0;
    return 1;
}

void drawCurlySnake(int **a, int locx, int locy, int snake)
{
    int i=0;
    a[locy][locx] = snake+1;
    for(locx++, i=1; i<3; i++, locx++)
        a[locy][locx] = snake;
    locx--;
    for(i=0; i<3; i++, locy++)
        a[locy][locx] = snake;
    locy--;
    for(i=0; i<2; i++, locx++)
        a[locy][locx] = snake;
    a[locy][locx] = snake+2;
}

void locateStraightSnake(int **a, int nos, int length, int x, int y) //nos: numberOfSnake
{
    locateStraight(a, nos, length, x, y, SNAKE3);
}

void locateLadder(int **a, int nol, int length, int x, int y) //nol: numberOfLadders 
{
    locateStraight(a, nol, length, x, y, LADDER);
}

//noi:numberOfItem --- length:lengthOfLadders --- x:xSizeOfBoard
void locateStraight(int **a, int noi, int length, int x, int y, int character)
{
    int locx=0, locy=0, tempx=0, tempy=0, keyx=0, keyy=0;
    
    for(; noi>0; noi--)
    {
        locx=rand()%x;
        tempx=locx;
        locy=rand()%(y-length+1);
        tempy=locy;
        
        while( controlStraight(a, length, tempx, tempy)!=1 )
        {
            if( keyx==0 )
            {
                tempx++;
                if( tempx>=x )
                {
                    keyx=1;
                    tempx=locx;
                }
            }
            if( keyx==1 )
            {
                tempx--;
                if( tempx<0 )
                {
                    keyx=0;
                    tempx=locx;
                    if( keyy==0 )
                    {
                        tempy--;
                        if( tempy<0 )
                        {
                            keyy=1;
                            tempy=locy;
                        }
                    }
                    if( keyy==1 )
                    {
                        tempy++;
                        if( (tempy+length-1)>=y )
                        {
                            printf("Exception happened when map creating.\n");
                            exit(0);
                        }
                    }
                }
            }
        }
        drawStraight(a, length, tempx, tempy, character);
    }
}

int controlStraight(int **a, int length, int locx, int locy)
{
    int i=0;
    
    for(i=0; i<length; i++, locy++)
        if( a[locy][locx]>300 )
            return 0;
    return 1;
}

void drawStraight(int **a, int length, int locx, int locy, int character)
{
    a[locy][locx] = character+1;
    for(locy++, length--; length>1; length--, locy++)
        a[locy][locx]=character;
    a[locy][locx]=character+2;
}

void initializeBoard(int **a, int x, int y)
{
    int i=0, j=0, counter=1, turn=0;
    
    for(i=y-1; i>=0; i--)
    {
        if( turn==0 )
            for(j=0, turn=1; j<x; j++)
            {
                a[i][j]=counter;
                counter++;
            }
        else
            for(j=x-1, turn=0; j>=0; j--)
            {
                a[i][j]=counter;
                counter++;
            }
    }
    
}

void displayBoard(int **a, int x, int y)
{
    int i=0, j=0;
    
    for(i=0; i<y; i++)
    {
        for(j=0; j<x; j++)
        {
            if( a[i][j]<300 )
                printf("%3d ", a[i][j]);
            else if( a[i][j]==SNAKE1 || a[i][j]==TAIL1 )
                printf("  a ");
            else if( a[i][j]==HEAD1 )
                printf("  A ");
            else if( a[i][j]==SNAKE2 || a[i][j]==TAIL2 )
                printf("  b ");
            else if( a[i][j]==HEAD2 )
                printf("  B ");
             else if( a[i][j]==SNAKE3 || a[i][j]==TAIL3 )
                printf("  c ");
            else if( a[i][j]==HEAD3 )
                printf("  C ");
            else if( a[i][j]==LADDER || a[i][j]==LFINIS || a[i][j]==LSTART )
                printf("  - ");
            else if( a[i][j]==PLAYER )
                printf("  @ ");
            else if( a[i][j]==COMPUTER )
                printf("  # ");
        }
        printf("\n");
    }
}

void displayResults(int winner, int rounds)
{
    if( winner==PLAYER )
        printf("Congratulations! The player has won :) The board is completed in %d rounds.\n", rounds);
    else
        printf("No! The computer has won :( The board is completed in %d rounds.\n", rounds);
}

int** create2Darray(int xAxis, int yAxis)
{
    int **a, i=0;
    a =(int **) malloc(yAxis*sizeof(int *));
    
    for(i=0; i<yAxis; i++)
        a[i] =(int *) malloc(xAxis*sizeof(int));
    
    return a;
}
{{< /highlight >}}
