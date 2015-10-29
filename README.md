# Project_Pandian
24. The G.L RESORTS is a Private limited Company which has a chain of resorts all over the state. It has a range of luxurious as well as economic rooms &amp; suites for the customers to stay in. When a Customers wants to  book rooms , he/she can view the room details and customer has to pay in  advance  10%of the total payment has to be made based on the room rent.  A booking request can be accepted only when vacancy is there according to the requirement. During check-out the total bill amount has to be cleared by the customer. Total bill amount should include the room rent, and 15% service tax of the total bill amount .A customer can book a room over phone and if within 5 hours of booking the customer does not checks in, booking is canceled and 2% from the advance payment is deducted. The company requests  you  to develop  a complete system to bring  automation for its room booking process.


 HMS2_cu.hpp
 
#ifndef HMS2_cu_hpp
#define HMS2_cu_hpp
class Customer
{
  public:
        bool connection();
        int login(char[],char[]);
        void registration(char[],char[],char[],double,char[],char[]);

        void book(int,char[]);
        void viewroomdetails();
        void cancellation(int);

};
#endif

**********************************************************************************
HMS2_man.hpp



#ifndef HMS2_man_hpp
#define HMS2_man_hpp
#include"HMS2_cu.hpp"
class Manager:public Customer
{
  public:

    void view(void);
    void payment(double);

};
#endif


***********************************************************************************
HMS2.pc


#include"HMS2_cu.hpp"
#include"HMS2_man.hpp"
#include<iostream>
#include<cstring>
#include<string.h>
#include<sqlda.h>
#include<sqlcpr.h>
#define NODATAFOUND 1403
using namespace std;
EXEC SQL INCLUDE SQLCA.H;

EXEC SQL BEGIN DECLARE SECTION;
char userId[20],d_date[20];
char pwd[20];
char x[20],a[30],b[30],i[20],j[20],k[20],l[20],p[30];
char y[20],m[30],date[20];
int d_room_no1,d_amt1,d_refund1;
char d_cust_name1[20],d_date1[20];
char d_uname[20],d_name[20];
char d_room_type[10],d_available[30];
int d_room_no,d_tariff,cmp1,cmp2,f=0,one_more,d_cancel;
char d_password[20];
char c_name[20];
int ch,ch2=1,cal,total,n_total;
char c_city[10];
double c_phone;
char dummy1[20],dummy2[20];
char gender[10];
EXEC SQL END DECLARE SECTION;

bool Customer::connection()
{
 strcpy(d_uname,"E1038416@UnixDB03");
 strcpy(d_password,"E1038416");
 EXEC SQL CONNECT :d_uname IDENTIFIED BY :d_password;
 if(sqlca.sqlcode != 0)
 return false;
 else
 return true;
}

int Customer::login(char user[20],char pass[20])
{
 if(connection())
 {
    strcpy(d_uname,user);
    strcpy(d_password,pass);




        EXEC SQL DECLARE Curs CURSOR FOR select uname,password  from cust_details;
        EXEC SQL OPEN Curs;
        for(;;)
        {
          EXEC SQL FETCH Curs INTO :a,:b;
          if(sqlca.sqlcode == NODATAFOUND)
          {
           break;
          }
          if(sqlca.sqlcode<0)
          {
          cout<<"ERROR";
          }
          else
          {
            cmp1=strcmp(a,d_uname);
            //cout<<a<<b<<d_uname<<d_password;
            cmp2=strcmp(b,d_password);
            if(cmp1==cmp2)
            {
                f=1;

            }
            else
            {
             f=0;
            }
         }
      }
  if(f==1)

 {
  cout<<"\n\t\tLogged in Successfull!!!\n";
    return 0;/*
do
{
  cout<<"Welcome Customer\n";
  cout<<"Press\n"<<"1. To view Rooms\n"<<"2.To Book a Room\n"<<"3.To cancel your booking\n"<<"\n4.To exit";
  cin>>ch;
  switch(ch)
     {
      case 1:
                cout<<"\nThese are available rooms";
                viewroomdetails();
                //cout<<"Please choose your room";
                break;
      case 2:

                cout<<"\nPlease enter your room no";
                cin>>d_room_no;
                book(d_room_no,d_uname);
                break;

      case 3:
                cout<<"\nEnter your room no";
                cin>>d_room_no;
                cancellation(d_room_no);
                break;

        default:
                ch2=0;
                break;
     }

}while(ch2==1);
*/ }
else
  {
  cout<<"not successful\n";
  return 1;
  }
 EXEC SQL COMMIT WORK RELEASE;
 }
 else
    {
        cout<<"Connection failed";
       }

}

void Customer::registration(char cusname[],char user[],char city[],double phone,char pass[],char sex[])
{
  if(connection())
  {

        strcpy(c_name,cusname);
        strcpy(d_uname,user);
        strcpy(c_city,city);
        c_phone = phone;
        strcpy(d_password,pass);
        strcpy(gender,sex);

        EXEC SQL Insert into cust_details values(:c_name,:d_uname,:c_city,:c_phone,:d_password,:gender);
        EXEC SQL COMMIT WORK RELEASE;
        cout<<"\n\t\tCustomer Details Added Successfully!!!"<<endl;
  }
   else
   {
         cout<<"Connection failed";
   }

}


void Customer::viewroomdetails()
{
  if(connection())
  {
strcpy(p,"AVAILABLE");
   EXEC SQL DECLARE Curs2 CURSOR for select * from Rooms where availability=:p;
   EXEC SQL OPEN Curs2;
   if(sqlca.sqlcode <0)
   {
    cout<<"Error in Execution";
   }
   for(;;)
   {
    EXEC SQL FETCH Curs2 Into :d_room_no,:d_room_type,:d_tariff,:d_available; //declare these local var later
    if(sqlca.sqlcode == NODATAFOUND)
{ break;}
    if(sqlca.sqlcode <0)
    {
     cout<<"Error in Execute";
    }
    cout<<"**********************************Room Details******************"<<endl;
    cout<<"\n\n\t\tRoom No : "<<d_room_no<<endl;//int
    cout<<"\n\t\tRoom Type : "<<d_room_type<<endl; //varchar
    cout<<"\n\t\tTariff: "<<d_tariff<<endl; //int
    cout<<"\n\t\tAvailability : "<<d_available<<endl;//varchar
    }
    cout<<"****************************************************************"<<endl;
    EXEC SQL COMMIT WORK RELEASE;

    }
    else
    {
      cout<<"Connection failed";
    }
}

void Customer::book(int room_id,char uname[])
{
if(connection())
  {
cout<<"\n\t\t Enter date of arrival:";
                cin>>date;
   d_room_no = room_id;
   strcpy(d_uname,uname);
        strcpy(m,"booked");
strcpy(d_date,date);
   EXEC SQL update Rooms set availability=:m where room_No=:d_room_no;
    EXEC SQL SELECT c_name into :d_name from cust_details where uname=:d_uname;
  cout<<"\n\t\tHow many days you want to stay?";
  cin>>one_more;

   if(sqlca.sqlcode <0)
   {
    cout<<"Error in Execution";
   }



}

 else
    {
      cout<<"Connection failed";
    }


 //Calculations.......................

  if(d_room_no>=101 && d_room_no<111)
  {
    cal=one_more*85;
    total= (850*one_more)-cal;
    n_total= 850*one_more*.15+total;


 }
else  if(d_room_no>=201 && d_room_no<211)
  {
    cal=one_more*150;
     total= (1500*one_more)-cal;
    n_total= 1500*one_more*.15+total;



  }
else
{
  cal=one_more*300;
    total= (3000*one_more)-cal;
    n_total= 3000*one_more*.15+total;


  }
EXEC SQL insert into man_details (room_no,cust_name,date_of_arr,amt_left,advance) values (:d_room_no,:d_name,:d_date,:n_total,:cal);

 EXEC SQL COMMIT WORK RELEASE;

  cout<<"\n\n\t\tYour room is booked now, You have to pay $"<<cal<<" as your advance booking fees";
 cout<<"\n\t\tYour total amount to pay $"<<n_total;

}



//From HMS3.pc

void Manager::view()
{
if(connection())
{
   EXEC SQL DECLARE Curs10 CURSOR for select * from man_details;
   EXEC SQL OPEN Curs10;
   if(sqlca.sqlcode <0)
   {



cout<<"Error in Execution";
   }
   for(;;)
   {
    EXEC SQL FETCH Curs10 Into :d_room_no1,:d_cust_name1,:d_date1,:d_amt1,:d_refund1;
    if(sqlca.sqlcode == NODATAFOUND)
    { break;}
    if(sqlca.sqlcode <0)
    {
     cout<<"Error in Execute";
    }
    cout<<"\n\n*****************************Room Details***********************"<<endl;
    cout<<"\n\tRoom No : "<<d_room_no1<<endl;//int
    cout<<"\n\tCustomer Name: "<<d_cust_name1<<endl; //varchar
    cout<<"\n\tDate of booking: "<<d_date1<<endl; //int
    cout<<"\n\tRefund : "<<d_refund1<<endl;//varchar
    }
    cout<<"******************************************************************"<<endl;
    EXEC SQL COMMIT WORK RELEASE;
}
else
{
      cout<<"Connection failed";
}

}




void Customer::cancellation(int room_id)
{
if(connection())
  {
        d_room_no=room_id;
        strcpy(m,"AVAILABLE");
        EXEC SQL update Rooms set availability=:m where room_No=:d_room_no;

   EXEC SQL DECLARE Curs11 CURSOR for select advance  from man_details where room_No=:d_room_no;
   EXEC SQL OPEN Curs11;

for(;;)
   {
    EXEC SQL FETCH Curs11 Into :d_refund1;
    if(sqlca.sqlcode == NODATAFOUND)
    { break;}
 if(sqlca.sqlcode <0)
   {
    cout<<"";
   }

        cout<<"\n\n\t\tRoom Booking cancelled Successfully"<<endl;
        cout<<"\n\tAdvance charges were : "<<d_refund1;
        d_cancel=d_refund1-(0.02*d_refund1);
        cout<<"\n\tYour refund afer cancellation is: "<<d_cancel;
}
   EXEC SQL delete from man_details where room_No=:d_room_no;
 EXEC SQL COMMIT WORK RELEASE;
}
 else
    {
      cout<<"Connection failed";
    }
}


********************************************************************************

HMS.cpp

#include"HMS2_cu.hpp"
#include"HMS2_man.hpp"
#include<iostream>
#include<string.h>
#include<cstring>

using namespace std;

int main()
{
  int x,x1=1,x3,ch2,ch,temp,room_no,cmp1,cmp2,f=0;
  double p_no;
  char id[20],pwd[20],name[20],city[10],g[10];
  Customer c; Manager m;
do
{
    cout<<"***************************************************************\n\n"<<endl;
    cout<<"\t\t\t\tWelcome to G L Resorts\n"<<endl;
    cout<<"Enter your choice.....\n"<<endl <<"\t\t\t1. Existing user? Login Here...."<<endl<<"\t\t\t2. New User? Register Here......"<<endl<<"\t\t\t3. Exit";
    cin>>ch;
    switch(ch)
    {
     case 1:
     {
       cout<<"\n\n\t\tLogin screen"<<endl;
       cout<<"\n\tEnter your registered email id"<<endl;
       cin>>id;
       cout<<"\n\tEnter your password"<<endl;
       cin>>pwd;
       if(!(strcmp(id,"manager")))
       {
           f=1;
           m.view();
           break;
       }


           temp= c.login(id,pwd);
           if(temp==0)
           {
           do
           {
             cout<<"\n\n\t\tWelcome Customer\n";
             cout<<"\n\t\tEnter your choice:\n"<<"\t1. To view Rooms\n"<<"\t2.To Book a Room\n"<<"\t3.To cancel your booking\n"<<"\t4.To exit\n";
             cin>>ch;
             switch(ch)
             {
              case 1:
              cout<<"\n\tThese are available rooms";

              c.viewroomdetails();
              break;
              case 2:
               cout<<"\n\tPlease enter your room no";
               cin>>room_no;
               c.book(room_no,id);
               break;

              case 3:
                cout<<"\n\tEnter your room no";
                 cin>>room_no;
                 c.cancellation(room_no);
                 break;


               default:
                  ch2=0;
                   break;
         }

      }while(ch2==1);
              }

  }
   break;
case 2:
    cout<<"\n\n\tEnter your Name "<<endl;
    cin>>name;
    cout<<"\n\tEnter your city"<<endl;
    cin>>city;
    cout<<"\n\tEnter your phone no"<<endl;
    cin>>p_no;
    cout<<"\n\tEnter your Gender"<<endl;
    cin>>g;
    cout<<"\n\tEnter your email id"<<endl;
    cin>>id;
    cout<<"\n\tEnter your password"<<endl;
    cin>>pwd;
    c.registration(name,id,city, p_no,pwd,g);
    break;

 default:
   x1=0;
   cout<<"\n\tEnter any valid no";
   break;
}

}while(x1==1);
return 0;
}

cin >> password;
        length = strlen(password);
    }

    if (testPass(password))
        cout << "Your password is valid." << endl;
    else
    {
        cout << "Your password is not valid. ";
        cout << "Please refer to the above warning message." << endl;
    }

    delete[] password ; // hey!! don't forget to free your allocated memory!!!
    return 0;
}

/*This function will check each input and ensure that the password
contains a uppercase, lowercase, and digit.*/

bool testPass(char pass[]) // a beautiful hack (:
{
	// flags
	bool aUpper = false,
		 aLower = false,
		 aDigit = false ;
	for ( int i = 0 ; pass[i] ; ++i )
		if ( isupper(pass[i]) )
			aUpper = true ;
		else if ( islower(pass[i]) )
			aLower = true ;
		else if ( isdigit(pass[i]) )
			aDigit = true ;
	if ( aUpper && aLower && aDigit )
		return true;
	else
		return false ;
}
