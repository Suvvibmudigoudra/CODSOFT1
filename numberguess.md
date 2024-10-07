# CODSOFT1
#include<stdio.h>
#include <bits/stdc++.h>
#include <iostream>
#include<cstdlib>
using namespace std;

int main()
{
    int sp, num_range, real_num, guess_num, guess_count = 0;
    start:
    cout<<"---------------------------------------------------------------"<<endl;
    cout<<"--------------- WELCOME TO NUMBER GUESSING GAME ----------------"<<endl;
    cout<<"----------------------------------------------------------------"<<endl;
    cout<<"Choose one option ---> "<<"\n"<<endl;
    cout<<"1.>>> Play the Game >>>"<<endl;
    cout<<"2.>>> See the last Game Score >>>"<<endl;
    cout<<"3.>>> Exit from the Game >>>"<<endl;
    cout<<"\n"<<"Enter Your Option : ";
    cin>>sp;
    switch(sp)
    {
        case 1:
            cout<<"\n"<<"Let's Start the Game..."<<"\n"<<endl;
            goto play;
            break;
        case 2:
            if(guess_count == 0)
            {
                cout<<"\n"<<"You haven't played the game once yet..."<<"\n"<<endl;
            }
            else if(guess_count == 1)
            {
                cout<<"\n"<<"In the Last Game, you won the game in "<<guess_count<<"st Try...!"<<"\n"<<endl;
            }
            else
            {
                cout<<"\n"<<"In the Last Game, you won the game in "<<guess_count<<" Tries..."<<"\n"<<endl;
            }
            goto start;
            break;
        case 3:
            cout<<"\n"<<"Thanks for Playing the Game...!"<<"\n"<<endl;
            exit(0);
            break;
        default:
            cout<<"\n"<<"Invalid Option Choice... Please Try Again..."<<"\n"<<endl;
            goto start;
    }
    play:
    guess_count = 1;
    cout<<"--------------------- NUMBER GUESSING GAME -------------------"<<endl;
    cout<<"\n"<<"Enter what would you like the range of the numbers to be? : ";
    cin>>num_range;
    srand(time(0));
    real_num = rand() % (num_range+1);
    do
    {
        cout<<"\n"<<"Enter your guess number : ";
        cin>>guess_num;
        if(guess_num > real_num)
        {
            guess_count += 1;
            cout<<"The number is lower than you guess...";
        }
        else if(guess_num < real_num)
        {
            guess_count += 1;
            cout<<"The number is higher than you guess...";
        }
        else
        {
            cout<<"\n"<<"Great.! You guessed the correct number...!!!";
            if(guess_count == 1)
            {
                cout<<"\n"<<"You guessed the number in "<< guess_count <<"st Try.!!!"<<"\n"<<endl;
            }
            else
            {
                cout<<"\n"<<"You guessed the number in "<< guess_count <<" Tries...!"<<"\n"<<endl;
            }
            goto start;
        }
    }
    while(guess_num != real_num);
    

    return 0;
}
