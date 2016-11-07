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

