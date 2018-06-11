# telephone-directory
# include&lt;fstream.h&gt;
#include&lt;stdio.h&gt;
#include&lt;string.h&gt;
#include&lt;process.h&gt;
#include&lt;ctype.h&gt;
#include&lt;iostream.h&gt;
class telephone
{
int id;
char name[30];
char
phone[15],address[30],alt_no[15];
public:
telephone();
telephone(telephone &amp;T);
void indata();
void outdata();
int getid();
char *getaddress();
char *getname();
char *getphone();

char*getalt_no();
};
telephone::telephone()
{
id=0;
name[0]=&#39;\0&#39;;
address[0]=&#39;\0&#39;;
phone[0]=&#39;\0&#39;;
alt_no[0]=&#39;\0&#39;;
}
telephone::telephone(telephone &amp;T)
{
id=T.id;
strcpy(name,T.name);
strcpy(address,T.address);
strcpy(phone,T.phone);
strcpy(alt_no,T.alt_no);
}
void telephone::indata()
{
cout&lt;&lt;&quot;enter id\n&quot;;
cin&gt;&gt;id;
cout&lt;&lt;&quot;enter name\n&quot;;

gets(name);
cout&lt;&lt;&quot;enter address\n&quot;;
gets(address);
cout&lt;&lt;&quot;enter phone no.\n&quot;;
gets(phone);
cout&lt;&lt;&quot;enter alternate number\n&quot;;
gets(alt_no);
}
void telephone::outdata()
{
cout&lt;&lt;&quot;id is\n&quot;&lt;&lt;id&lt;&lt;&quot;name is\n&quot;;
puts(name);
cout&lt;&lt;&quot;address is\n&quot;;puts(address);
cout&lt;&lt;&quot;phoen no is\n&quot;;puts(phone);
cout&lt;&lt;&quot;alternate number
is\n&quot;;puts(alt_no);
}
int telephone::getid()
{return(id);}
char* telephone::getname()
{return(name);}
char* telephone::getaddress()
{

return(address);
}
char* telephone::getphone()
{
return(phone);
}
char* telephone::getalt_no()
{
return(alt_no);
}
void WRITE()
{
ofstream
f(&quot;telephone.dat&quot;,ios::binary|ios::app
);
telephone t;
char reply;
do
{
t.indata();
f.write((char*)&amp;t,sizeof(t));
cout&lt;&lt;&quot;enter more records\n&quot;;
cin&gt;&gt;reply;

}while(toupper(reply)==&#39;Y&#39;);
f.close();
}
void READ()
{
ifstream
f(&quot;telephone.dat&quot;,ios::binary|ios::in);
telephone t;
if(!f)
{
cout&lt;&lt;&quot;file does not exist\n&quot;;
return;
}
int ctr=0;
while(f.read((char*)&amp;t,sizeof(t)))
{
cout&lt;&lt;&quot;\nrecord:&quot;&lt;&lt;++ctr;
t.outdata();
}
f.close();
}
void SEARCH_ID()
{

ifstream
f(&quot;telephone.dat&quot;,ios::in|ios::binary);
char found=&#39;N&#39;;
if(!f)
{
cout&lt;&lt;&quot;file reading error\n&quot;;
return;
}
telephone t;
int idno;
cout&lt;&lt;&quot;enter id to searched\n&quot;;
cin&gt;&gt;idno;
while(f.read((char*)&amp;t,sizeof(t)))
{
if(t.getid()==idno)
{
cout&lt;&lt;&quot;information is\n&quot;;
t.outdata();
found=&#39;Y&#39;;
break;
}
}
if(found==&#39;N&#39;)

cout&lt;&lt;&quot;no such record found\n&quot;;
f.close();
}
void SORT_ENO()
{
fstream f;
f.open(&quot;telephone.dat&quot;,ios::binary|ios
::in);
telephone t[40];
if(!f)
{
cout&lt;&lt;&quot;file does not exist\n&quot;;
return;
}
int n=0;
while(f.read((char*)&amp;t[n],sizeof(t)))
{
n++;
telephone temp;
int i,j;
for(i=1;i&lt;n;i++)
for(j=1;j&lt;n-i;j++)
if(t[j].getid()&gt;t[j+1].getid())

{
temp=t[j];
t[j]=t[j+1];
t[j+1]=temp;
}
}
f.close();
f.open(&quot;telephone.dat&quot;,ios::out|ios::b
inary);
int i=0;
while(i&lt;n)
{
f.write((char*)&amp;t[i],sizeof(t));
++i;
}
cout&lt;&lt;&quot;file sorted\n&quot;;
f.close();
}
void SEARCH_NAME()
{
ifstream
f(&quot;telephone.dat&quot;,ios::in|ios::binary);
char found=&#39;N&#39;;

if(!f)
{
cout&lt;&lt;&quot;file reading error\n&quot;;
return;
}
telephone t;
char namee[30];
cout&lt;&lt;&quot;enter name to be
searched\n&quot;;
gets(namee);
while(f.read((char*)&amp;t,sizeof(t)))
{
if((strcmp(t.getname(),namee))==0)
{
t.outdata();
found=&#39;Y&#39;;
}
}
if(found==&#39;N&#39;)
cout&lt;&lt;&quot;no record found with such
name\n&quot;;
f.close();
}

void insertion()
{
ifstream
fmain(&quot;telephone.dat&quot;,ios::binary|ios:
:in);
ofstream
ftemp(&quot;temp.dat&quot;,ios::binary|ios::out)
;
telephone t,tnew;
cout&lt;&lt;&quot;enter new record
information\n&quot;;
tnew.indata();
char EOF_FLAG=&#39;Y&#39;;
while(fmain.read((char*)&amp;t,sizeof(t)))
{
if(t.getid()&lt;=tnew.getid())
ftemp.write((char*)&amp;t,sizeof(t));
else
{
ftemp.write((char*)&amp;tnew,sizeof(tnew)
);
EOF_FLAG=&#39;N&#39;;
break;

}
}
if(EOF_FLAG==&#39;Y&#39;)
ftemp.write((char*)&amp;tnew,sizeof(tnew)
);
else
while(!fmain.eof())
{
ftemp.write((char*)&amp;t,sizeof(t));
fmain.read((char*)&amp;t,sizeof(t));
}
fmain.close();
ftemp.close();
remove(&quot;telephone.dat&quot;);
rename(&quot;temp.dat&quot;,&quot;telephone.dat&quot;);
}
void MODIFY()
{
fstream
f(&quot;telephone.dat&quot;,ios::in|ios::out|ios::
binary);
telephone t;
int idno;

char found;
cout&lt;&lt;&quot;enter id whose record is to be
modified\n&quot;;
cin&gt;&gt;idno;
int rec_count=0;
while(f.read((char*)&amp;t,sizeof(t)))
{
if(t.getid()==idno)
{
cout&lt;&lt;&quot;enter new information\n&quot;;
t.indata();
f.seekg(rec_count*sizeof(t),ios::beg);
f.write((char*)&amp;t,sizeof(t));
found=&#39;Y&#39;;
break;
}
rec_count++;
}
if(found==&#39;Y&#39;)
cout&lt;&lt;&quot;record updated\n&quot;;
else
cout&lt;&lt;&quot;\n record not found\n&quot;;
f.close();

}
void deletion()
{
ifstream
fmain(&quot;emp.dat&quot;,ios::binary|ios::in);
ofstream
ftemp(&quot;temp.dat&quot;,ios::binary|ios::out)
;
telephone t;
int idno;
cout&lt;&lt;&quot;enter id whose contact is to
be deleted\n&quot;;
cin&gt;&gt;idno;
char found=&#39;Y&#39;;
while(fmain.read((char*)&amp;t,sizeof(t)))
{
if(t.getid()!=idno)
ftemp.write((char*)&amp;t,sizeof(t));
else
found=&#39;Y&#39;;
}
if(found==&#39;N&#39;)
cout&lt;&lt;&quot;record not found\n&quot;;

fmain.close();
ftemp.close();
remove(&quot;telephone.dat&quot;);
rename(&quot;temp.dat&quot;,&quot;telephone.dat&quot;);
}
void main()
{
int ch;
do
{
cout&lt;&lt;&quot;\n\n\nfile operation menu\n&quot;;
cout&lt;&lt;&quot;1-to create or append file\n2-
to display all records\n3-display
record of a particular id\n4-display
record for a name\n&quot;;
cout&lt;&lt;&quot;5-insertion of new record in
order of id\n6-deletion of a record of
an id\n7-modification of record of an
id\n 8-to exit\n&quot;;
cin&gt;&gt;ch;
switch(ch)
{
case 1:WRITE();break;

case 2:READ();break;
case 3:SEARCH_ID();break;
case 4:SEARCH_NAME();break;
case
5:SORT_ENO();insertion();break;
case 6:deletion();break;
case 7:MODIFY();break;
case 8:exit(0);
}
}while(1);
}
