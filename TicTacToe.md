#include<bits/stdc++.h>
#include<iostream>
#include<string>
#include<map>

using namespace std;

int analyze_board(int board[]);

void ConstBoard(int board[])
{
    cout<<endl<<"Current State of the Board : "<<endl<<endl<<endl;
    for(int i=0; i<9; i++)
    {
        if((i > 0) and (i%3 == 0))
        {
            cout<<endl<<endl;
        }
        if(board[i] == 0)
        {
            cout<<" _  ";
        }
        if(board[i] == -1)
        {
            cout<<" X  ";
        }
        if(board[i] == 1)
        {
            cout<<" O  ";
        }
    }
    cout<<endl<<endl<<endl;
}

void User1Turn(int board[])
{
    int pos;
    cout<<"Enter X's position from [1,2,3,.....,9] : ";
    cin>>pos;
    if(board[pos-1] != 0)
    {
        cout<<endl<<"Wrong Move.!!!"<<endl;
        // board[pos-1] = 1;
        ConstBoard(board);
        User1Turn(board);
    }
    else
    {
        board[pos-1] = -1;
    }
}

void User2Turn(int board[])
{
    int pos;
    cout<<"Enter O's position from [1,2,3,.....,9] : ";
    cin>>pos;
    if(board[pos-1] != 0)
    {
        cout<<endl<<"Wrong Move.!!!"<<endl;
        ConstBoard(board);
        User2Turn(board);
    }
    else
    {
        board[pos-1] = 1;
    }
}

int min_max(int board[], int player)
{
    int x = analyze_board(board);
    if(x != 0)
    {
        return (x*player);
    }
    int pos = -1;
    int value = -2;
    for(int i=0; i<9; i++)
    {
        if(board[i] == 0)
        {
            board[i] = player;
            int score = -min_max(board, (player*-1));
            board[i] = 0;
            if(score > value)
            {
                value = score;
                pos = i;
            }
        }
    }
    if(pos == -1)
    {
        return 0;
    }
    return value;
}

void CompTurn(int board[])
{
    int pos = -1;
    int value = -2;
    for(int i=0; i<9; i++)
    {
        if(board[i] == 0)
        {
            board[i] = 1;
            int score = -min_max(board, -1);
            board[i] = 0;
            if(score > value)
            {
                value = score;
                pos = i;
            }
        }
    }
    board[pos] = 1;
}

int analyze_board(int board[])
{
    int cb[8][3] = {{0, 1, 2}, {3, 4, 5}, {6, 7, 8}, {0, 3, 6}, {1, 4, 7}, {2, 5, 8}, {0, 4, 8}, {2, 4, 6}};
    
    for(int i=0; i<8; i++)
    {
        if(board[cb[i][0]]!=0 and board[cb[i][0]]==board[cb[i][1]] and board[cb[i][0]]==board[cb[i][2]])
        {
            return board[cb[i][0]];
        }
    }
    return 0;
}

void game_instruction()
{
    cout<<"------------------ WELCOME TO TIC-TAC-TOE GAME INSTRUCTIONS -----------------"<<endl;
    cout<<endl<<"Demo Board --->"<<endl<<endl;
    cout<<" 1   2   3 "<<endl<<endl;
    cout<<" 4   5   6 "<<endl<<endl;
    cout<<" 7   8   9 "<<endl<<endl;
    cout<<endl<<"Game Instructions --->"<<endl<<endl;
    cout<<"For putting X or O in the board, you "<<endl;
    cout<<"have to enter board position numbers "<<endl;
    cout<<"[ 1,  2,  3,  4,  5,  6,  7,  8,  9] "<<endl;
    cout<<"shown in the above demo board..."<<endl<<endl;
}

int main()
{
    int choice, player;
    int board[9] = {0, 0, 0, 0, 0, 0, 0, 0, 0};
    
    cout<<endl;
    cout<<"------------------------ WELCOME TO TIC-TAC-TOE GAME ------------------------"<<endl;
    cout<<"Select any choice --->"<<endl<<endl;
    cout<<"1.>>> Show Tic-Tac-Toe Game Instructions >>>"<<endl;
    cout<<"2.>>> Single-Player (Play with Computer) >>>"<<endl;
    cout<<"3.>>> Multi-Player (Play with a Friend)  >>>"<<endl;
    cout<<"4.>>> Exit >>>"<<endl;
    cout<<endl<<"Enter your choice : ";
    cin>>choice;
    switch(choice)
    {
        case 1:
            cout<<endl<<"Showing how to play the game..."<<endl<<endl;
            game_instruction();
            main();
            break;
        case 2:
            cout<<endl<<"Computer: O Vs. You: X"<<endl;
            cout<<endl<<"Enter to Play 1(st) or 2(nd) : ";
            cin>>player;
            for(int i=0; i<9; i++)
            {
                if(analyze_board(board) != 0)
                {
                    break;
                }
                if((i+player)%2 == 0)
                {
                    CompTurn(board);
                }
                else
                {
                    ConstBoard(board);
                    User1Turn(board);
                }
            }
            break;
        case 3:
            for(int i=0; i<9; i++)
            {
                if(analyze_board(board) != 0)
                {
                    break;
                }
                if(i%2 == 0)
                {
                    ConstBoard(board);
                    User1Turn(board);
                }
                else
                {
                    ConstBoard(board);
                    User2Turn(board);
                }
            }
            break;
        case 4:
            cout<<endl<<"Exiting..."<<endl;
            cout<<endl<<"Thank You for playing the game..."<<endl<<endl;
            exit(0);
            break;
        default:
            cout<<endl<<"Invalid choice...! Please try again..."<<endl;
            main();
            break;
    }
    
    int x = analyze_board(board);
    if(x == 0)
    {
        ConstBoard(board);
        cout<<endl<<"It's a Draw...!!!"<<endl<<endl;
        main();
    }
    else if(x == -1)
    {
        ConstBoard(board);
        cout<<endl<<"Player X Wins...!!! O Looses.!"<<endl<<endl;
        main();
    }
    else if(x == 1)
    {
        ConstBoard(board);
        cout<<endl<<"Player O Wins...!!! X Looses.!"<<endl<<endl;
        main();
    }
}


