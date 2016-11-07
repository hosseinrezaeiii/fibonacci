----------------------------------------------------------------------------------
-- Asynchronous Circuit Design Final Project
-- Design and Implementation of an Asynchronous Fibonacci Series Generator
-- Hossein Rezaei, 93611133, (IUST)
-- hossein.rezaei991@gmail.com
-- Create Date:    22:57:26 01/02/2016 
-- version3 (ncu)
--***********************************************

-- The main Code

library IEEE;
use IEEE.STD_LOGIC_1164.ALL; --- The IEEE 1164 standard defines a package design unit that contains declarations that support a uniform representation of a logic value in a VHDL hardware description.
library UNISIM;
use UNISIM.VComponents.all; ---Using this declaration, the simulator references the functional models for all device primitives. In addition to this declaration, you must compile the library and map the library to the simulator.

entity fibonacci is
port(dout: out std_logic_vector(7 downto 0);
	  req:  in  std_logic;
	  ack:  out std_logic;
	  rst:  in  std_logic);
end fibonacci;

architecture Behavioral of fibonacci is
attribute keep_hierarchy : string;
attribute keep_hierarchy of
Behavioral: architecture is "true";
attribute keep : string;
signal rx,ry,rc,rf,ax,ay,ac,af,ain: std_logic;
signal temp1,temp2,trf,temprf: std_logic:='0';
signal tempx: std_logic_vector(7 downto 0);
signal tempy: std_logic_vector(7 downto 0);
signal tempf: std_logic_vector(7 downto 0);
signal tempc: std_logic_vector(7 downto 0) ;
attribute keep of rx,ry,rc,rf,ax,ay,ac,af,ain: signal is "true";
component latch port(
din:  in  std_logic_vector(7 downto 0);
dout: out std_logic_vector(7 downto 0);
req:  in  std_logic;
rst:  in  std_logic;
ack:  out std_logic);
end component;
attribute keep of latch : component is "true";
component adder port(
a: in std_logic_vector(7 downto 0);
b: in std_logic_vector(7 downto 0);
sum: out std_logic_vector(7 downto 0);
ack: out std_logic;
rst: in  std_logic;
req: in  std_logic);
end component;
attribute keep of adder : component is "true";
component CU port  
(rst: in std_logic;
rin: in std_logic;
rx: out std_logic;
ry: out std_logic;
rc: out std_logic;
rf: out std_logic;
ain: out std_logic;
ax: in std_logic;
ay: in std_logic;
ac: in std_logic;
af: in std_logic);
end component;
attribute keep of CU : component is "true";
begin
			
x: latch port map(
din   => tempf,
dout  => tempx,
req   => rx,
rst   => rst,
ack   => ax);
						 		
y: latch port map(
din   => tempx,
dout  => tempy,
req   => ry,
rst   => rst,
ack   => ay);
		
f: latch port map(
din   => tempc,
dout  => tempf,
req   => rf,
rst   => rst,
ack   => af);
						 
c: adder port map(
a    => tempx, 
b    => tempy,
sum  => tempc,
ack  => ac,
rst  => rst,
req  => rc);

u: CU port map( 
rst => rst,
rin => req,  
rx  => rx,
ry  => ry,
rc  => rc,
rf  => rf,
ain => ain,
ax  => ax,
ay  => ay,
ac  => ac,
af  => af);		
	  
dout <= tempf;
ack  <= ain;

end Behavioral;



--***********The components of the code****************

------The latch component
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity latch is port
(din:  in  std_logic_vector(7 downto 0);
dout: out std_logic_vector(7 downto 0);
req:  in  std_logic;
rst:  in  std_logic;
ack:  out std_logic);
end latch;

architecture Behavioral of latch is
begin
process(req,rst)
begin
if (rst='0') then
	if (rising_edge(req)) then
	 dout <= din;
	end if;
elsif (rst='1') then
   dout <= x"01";
end if;
if req='1' then
 ack <='1';
else
 ack <='0';
end if;
end process;
end Behavioral;
--***************************************
---------The control unit (CU) component

library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
library UNISIM;
use UNISIM.VComponents.all;

entity CU is port( 
rst: in std_logic;
rin: in std_logic;
rx: out std_logic;
ry: out std_logic;
rc: out std_logic;
rf: out std_logic;
ain: out std_logic;
ax: in std_logic;
ay: in std_logic;
ac: in std_logic;
af: in std_logic);
end CU;

architecture Behavioral of CU is
signal l0,l1,tain,l3,l31,l4,trx,l6,try,l8,l9,trc,trf,l12,csc0,l14,csc2,l16,l17,l18,l181,csc4,csc1,csc3:std_logic;
attribute s: string;
attribute s of l0,l1,tain,l3,l31,l4,trx,l6,try,l8,l9,trc,trf,l12,csc0,l14,csc2,l16,l17,l18,l181,csc4,csc1,csc3: signal is "yes";
attribute keep: string;
attribute keep of l0,l1,tain,l3,l31,l4,trx,l6,try,l8,l9,trc,trf,l12,csc0,l14,csc2,l16,l17,l18,l181,csc4,csc1,csc3: signal is "true";
begin
--p0
   p0 : LUT4 generic map (INIT => X"d500")
   port map (
      O  => l0,   
      I0 => trx, 
      I1 => csc4, 
      I2 => csc3, 
      I3 => csc0);
--p1
   p1 : LUT4 generic map (INIT => X"0001")
   port map (
      O  => l1,   
      I0 => csc1, 
      I1 => csc0, 
      I2 => rin, 
      I3 => rst);
--p2
   p2 : LUT4 generic map (INIT => X"00b2")
   port map (
      O  => tain,   
      I0 => tain, 
      I1 => l1, 
      I2 => l0, 
      I3 => rst);
--p3
   p3a : LUT4 generic map (INIT => X"0008")
   port map (
      O  => l31,   
      I0 => csc2, 
      I1 => csc1, 
      I2 => csc0, 
      I3 => tain);
   p3 : LUT2 generic map (INIT => X"4")
   port map (
      O  => l3,   
      I0 => csc3, 
      I1 => l31);
--p4
   p4 : LUT2 generic map (INIT => X"8")
   port map (
      O  => l4,   
      I0 => csc3, 
      I1 => csc4);
--p5
   p5 : LUT4 generic map (INIT => X"00b2")
   port map (
      O  => trx,   
      I0 => trx, 
      I1 => l4, 
      I2 => l3, 
      I3 => rst);
--p6
   p6 : LUT3 generic map (INIT => X"40")
   port map (
      O  => l6,   
      I0 => csc2, 
      I1 => csc1, 
      I2 => rin);
--p7
   p7 : LUT4 generic map (INIT => X"00b2")
   port map (
      O  => try,   
      I0 => try, 
      I1 => csc2, 
      I2 => l6, 
      I3 => rst);
--p8
   p8 : LUT3 generic map (INIT => X"20")
   port map (
      O  => l8,   
      I0 => csc0, 
      I1 => ac, 
      I2 => ax);
--p9
   p9 : LUT4 generic map (INIT => X"0057")
   port map (
      O  => l9,   
      I0 => ax, 
      I1 => csc3, 
      I2 => csc0, 
      I3 => rst);
--p10
   p10 : LUT4 generic map (INIT => X"00b2")
   port map (
      O  => trc,   
      I0 => trc, 
      I1 => l9, 
      I2 => l8, 
      I3 => rst);
--p11
   p11 : LUT3 generic map (INIT => X"04")
   port map (
      O  => trf,   
      I0 => csc4, 
      I1 => ac, 
      I2 => rst);
--p12
   p12 : LUT3 generic map (INIT => X"04")
   port map (
      O  => l12,   
      I0 => trc, 
      I1 => trx, 
      I2 => rst);
--p13
   p13 : LUT4 generic map (INIT => X"00b2")
   port map (
      O  => csc0,   
      I0 => csc0, 
      I1 => tain, 
      I2 => l12, 
      I3 => rst);
--p14
   p14 : LUT4 generic map (INIT => X"0001")
   port map (
      O  => l14,   
      I0 => ay, 
      I1 => csc1, 
      I2 => tain, 
      I3 => rst);
--p15
   p15 : LUT4 generic map (INIT => X"00b2")
   port map (
      O  => csc2,   
      I0 => csc2, 
      I1 => l14, 
      I2 => ay, 
      I3 => rst);
--p16
   p16 : LUT3 generic map (INIT => X"04")
   port map (
      O  => l16,   
      I0 => csc4, 
      I1 => trf, 
      I2 => rst);
--p17
   p17 : LUT2 generic map (INIT => X"8")
   port map (
      O  => l17,   
      I0 => csc3, 
      I1 => af);
--p18
   p18a : LUT2 generic map (INIT => X"1")
   port map (
      O  => l181,   
      I0 => csc3, 
      I1 => af);
   p18 : LUT4 generic map (INIT => X"8000")
   port map (
      O  => l18,   
      I0 => l181, 
      I1 => csc0, 
      I2 => trc, 
      I3 => trx);
--p19
   p19 : LUT4 generic map (INIT => X"00b2")
   port map (
      O  => csc4,   
      I0 => csc4, 
      I1 => l18, 
      I2 => l17, 
      I3 => rst);
--p20
   p20 : LUT4 generic map (INIT => X"002b")
   port map (
      O  => csc1,   
      I0 => csc1, 
      I1 => csc2, 
      I2 => tain, 
      I3 => rst);
--p21
   p21 : LUT4 generic map (INIT => X"00e8")
   port map (
      O  => csc3,   
      I0 => csc3, 
      I1 => ax, 
      I2 => l16, 
      I3 => rst);
ain<= tain;
rx <= trx;
ry <= try;
rc <= trc;
rf <= trf;		
end Behavioral;
--***********************
------------The adder component
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
library UNISIM;
use UNISIM.VComponents.all;

entity adder is port(
a: in std_logic_vector(7 downto 0);
b: in std_logic_vector(7 downto 0);
sum: out std_logic_vector(7 downto 0);
req: in  std_logic;
ack: out std_logic;
rst: in  std_logic);
end adder;

architecture Behavioral of adder is
signal cin0, cin1, cin2, cin3, cin4, cin5, cin6, cin7: std_logic_vector(1 downto 0);
signal cout0, cout1, cout2, cout3, cout4, cout5, cout6, cout7: std_logic_vector(1 downto 0);
signal ack0, ack1, ack2, ack3, ack4, ack5, ack6, ack7: std_logic;
signal nstart: std_logic;
signal s: std_logic_vector(7 downto 0);
signal l1, l2, l3, l4, l5, l6, l7, l8: std_logic :='0';
attribute keep : string;
attribute keep of l1, l2, l3, l4, l5, l6, l7, l8 : signal is "true";
component onebifulladder port ( a, bb, nstart :in std_logic; 
                               cin:  in  std_logic_vector(1 downto 0);
	                          	 cout: out std_logic_vector(1 downto 0);
                               sum:  out std_logic);
end component;
begin
cin0<="01";
nstart<= '1' when rst='1' else (NOT req);
bitadder0 : onebifulladder port map (
	   a => a(0),
		bb => b(0),
		nstart => nstart,
      cin  => cin0,
		cout => cout0,
      sum  => s(0));
ack0 <= cout0(0) XOR cout0(1);
cin1 <= cout0;
bitadder1 : onebifulladder port map (
	   a => a(1),
		bb => b(1),
		nstart => nstart,
      cin  => cin1,
		cout => cout1,
      sum  => s(1));
ack1 <= cout1(0) XOR cout1(1);
cin2 <= cout1;
bitadder2 : onebifulladder port map (
	   a => a(2),
		bb => b(2),
		nstart => nstart,
      cin  => cin2,
		cout => cout2,
      sum  => s(2));
ack2 <= cout2(0) XOR cout2(1);
cin3 <= cout2;
bitadder3 : onebifulladder port map (
	   a => a(3),
		bb => b(3),
		nstart => nstart,
      cin  => cin3,
		cout => cout3,
      sum  => s(3));
ack3 <= cout3(0) XOR cout3(1);
cin4 <= cout3;
bitadder4 : onebifulladder port map (
	   a => a(4),
		bb => b(4),
		nstart => nstart,
      cin  => cin4,
		cout => cout4,
      sum  => s(4));
ack4 <= cout4(0) XOR cout4(1);
cin5 <= cout4;
bitadder5 : onebifulladder port map (
	   a => a(5),
		bb => b(5),
		nstart => nstart,
      cin  => cin5,
		cout => cout5,
      sum  => s(5));
ack5 <= cout5(0) XOR cout5(1);
cin6 <= cout5;
bitadder6 : onebifulladder port map (
	   a => a(6),
		bb => b(6),
		nstart => nstart,
      cin  => cin6,
		cout => cout6,
      sum  => s(6));	
ack6 <= cout6(0) XOR cout6(1);
cin7 <= cout6;		
bitadder7 : onebifulladder port map (
	   a => a(7),
		bb => b(7),
		nstart => nstart,
      cin  => cin7,
		cout => cout7,
      sum  => s(7));
ack7 <= cout7(0) XOR cout7(1);
--ack generation-- need compelition detection
mullerC2 :  LUT3 generic map (INIT =>x"e8")
   port map (
      O  => l2,     
      I0 => l2,    
      I1 => ack0, 
      I2 => ack1 );
mullerC3 :  LUT3 generic map (INIT =>x"e8")
   port map (
      O  => l3,     
      I0 => l3,    
      I1 => ack2, 
      I2 => ack3 );
mullerC4 :  LUT3 generic map (INIT =>x"e8")
   port map (
      O  => l4,     
      I0 => l4,    
      I1 => ack4, 
      I2 => ack5 );
mullerC5 :  LUT3 generic map (INIT =>x"e8")
   port map (
      O  => l5,     
      I0 => l5,    
      I1 => ack6, 
      I2 => ack7 );
mullerC6 :  LUT3 generic map (INIT =>x"e8")
   port map (
      O  => l6,     
      I0 => l6,    
      I1 => l2, 
      I2 => l3 );
mullerC7 :  LUT3 generic map (INIT =>x"e8")
   port map (
      O  => l7,     
      I0 => l7,    
      I1 => l4, 
      I2 => l5 );
mullerC8 :  LUT3 generic map (INIT =>x"e8")
   port map (
      O  => l8,     
      I0 => l8,    
      I1 => l6, 
      I2 => l7 );
ack <= '0' when rst='1' else l8;
sum <= x"00" when rst='1' else s;
end Behavioral;
--*****************************


---------The one bit full adder component
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use ieee.numeric_std.all;
use IEEE.STD_LOGIC_unsigned.ALL;
Library UNISIM;
use UNISIM.vcomponents.all;
entity onebifulladder is
port( a, bb, nstart :in std_logic; 
      cin:  in  std_logic_vector(1 downto 0);
		cout: out std_logic_vector(1 downto 0);
      sum:  out std_logic);
end onebifulladder;

architecture Behavioral of onebifulladder is
signal a0, a1, b0, b1, hs0, hs1: std_logic;
component singletodual port (a, b, nstart: in std_logic ; a0, a1, b0, b1: out std_logic);
end component;
component MXD port (a0,a1,hs0,hs1,cin0,cin1 :in std_logic; cout0, cout1 :out std_logic);
end component;
component dualrailXOR port (a0, a1, b0, b1: in std_logic ; hs0, hs1:out std_logic);
end component;
begin
b : singletodual port map (
      a0 => a0,   
      a1 => a1,   
      b0 => b0,     
      b1 => b1,    
	   a => a,
		b => bb,
		nstart => nstart );
c : MXD port map (
      cout0 => cout(0),   
      cout1 => Cout(1),   
      a0 => a0,     
      a1 => a1,    
	   hs0 => hs0,
		hs1 => hs1,
		cin0 => cin(0),
		cin1 => cin(1));
d : dualrailXOR port map (
      hs0 => hs0,   
      hs1 => hs1,   
      a0 => a0,     
      a1 => a1,    
	   b0 => b0,
		b1 => b1 );
sum <= hs1 XOR cin(1);
end Behavioral;
--*******************************


------The single to dual component
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use ieee.numeric_std.all;
use IEEE.STD_LOGIC_unsigned.ALL;
Library UNISIM;
use UNISIM.vcomponents.all;
entity singletodual is port(a, b, nstart: in std_logic ; a0, a1, b0, b1: out std_logic);
end singletodual;
architecture behav of singletodual is
begin
--n refers to NOR
  n1 :  LUT2 generic map (INIT =>x"2")
   port map (
      O  =>a1,    
      I0 =>a,    
      I1 =>nstart);
  n2 :  LUT2 generic map (INIT =>x"1")
   port map (
      O  =>a0,    
      I0 =>a,    
      I1 =>nstart);
  n3 :  LUT2 generic map (INIT =>x"2")
   port map (
      O  =>b1,    
      I0 =>b,    
      I1 =>nstart);
  n4 :  LUT2 generic map (INIT =>x"1")
   port map (
      O  =>b0,    
      I0 =>b,    
      I1 =>nstart);
end behav;
--****************************


--------The dual-rail XOR component
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use ieee.numeric_std.all;
use IEEE.STD_LOGIC_unsigned.ALL;
Library UNISIM;
use UNISIM.vcomponents.all;
entity dualrailXOR is port(a0, a1, b0, b1: in std_logic ; hs0, hs1:out std_logic);
end dualrailXOR;
architecture behav of dualrailXOR is
signal l1,l2,l3,l4 :std_logic:='0';
attribute keep : string;
attribute keep of l1 : signal is "true";--my ref clk is the name of signalto be kept
attribute keep of l2 : signal is "true";
attribute keep of l3 : signal is "true";
attribute keep of l4: signal is "true";
begin
 muller1 :  LUT3 generic map (INIT =>x"e8")
   port map (
      O  =>l1,    
      I0 =>l1,    
      I1 =>a1, 
      I2 =>b0);
  muller2 :  LUT3 generic map (INIT =>x"e8")
   port map (
      O  =>l2,    
      I0 =>l2,    
      I1 =>a0, 
      I2 =>b1);
  muller3 :  LUT3 generic map (INIT =>x"e8")
   port map (
      O  =>l3,    
      I0 =>l3,    
      I1 =>a1, 
      I2 =>b1);
  muller4 :  LUT3 generic map (INIT =>x"e8")
   port map (
      O  =>l4,    
      I0 =>l4,    
      I1 =>a0, 
      I2 =>b0);
	hs1<=l1 or l2;
	hs0<=l3 or l4;
end behav;
--*********************************


---------The MXD component
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use ieee.numeric_std.all;
use IEEE.STD_LOGIC_unsigned.ALL;
Library UNISIM;
use UNISIM.vcomponents.all;
entity MXD is port(a0,a1,hs0,hs1,cin0,cin1 :in std_logic; cout0, cout1 :out std_logic);
end MXD;
architecture behav of MXD is
signal l1,l2: std_logic:='0';
attribute keep : string;
attribute keep of l1 : signal is "true";
attribute keep of l2 : signal is "true";
begin
cinblock0 :  LUT4 generic map (INIT =>x"f888")
   port map (
      O  =>l1,   
      I0 =>hs1,   
      I1 =>cin0,     
      I2 =>hs0,    
	   I3 =>a0);
cinblock1 :  LUT4 generic map (INIT =>x"f888")
   port map (
      O  =>l2,     
      I0 =>cin1,   
      I1 =>hs1,     
      I2 =>a1,     
	   I3 =>hs0);
cout0<=l1;
cout1<=l2;
end behav;
--***************************************************

