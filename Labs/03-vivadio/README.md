# Lab 3: Introduction to Vivado

## 1. Preparation

### Table with connection of 16 slide switches and 16 LEDs on Nexys A7 board

| **LED** | **connection** | **x** | **Switch** | **connection** |
| :---: | :---: | :---: | :---: | :---: |
| LED 0 | H17 |  | SW 0 | J15 |
| LED 1 | K15 |  | SW 1 | L16 |
| LED 2 | J13 |  | SW 2 | M13 |
| LED 3 | N14 |  | SW 3 | R15 |
| LED 4 | R18 |  | SW 4 | R17 |
| LED 5 | V17 |  | SW 5 | T18 |
| LED 6 | U17 |  | SW 6 | U18 |
| LED 7 | U16 |  | SW 7 | R13 |
| LED 8 | V16 |  | SW 8 | T8 |
| LED 9 | T15 |  | SW 9 | U8 |
| LED 10 | U14 |  | SW 10 | R16 |
| LED 11 | T16 |  | SW 11 | T13 |
| LED 12 | V15 |  | SW 12 | H6 |
| LED 13 | V14 |  | SW 13 | U12 |
| LED 14 | V12 |  | SW 14 | U11 |
| LED 15 | V11 |  | SW 15 | V10 |

## 2. Two-bit wide 4-to-1 multiplexer

### VHDL architecture from source file mux_2bit_4to1.vhd

```VHDL
architecture Behavioral of mux_2bit_4to1 is
begin

    f_o <=  a_i when (sel_i = "00") else
            b_i when (sel_i = "01") else
            c_i when (sel_i = "10") else
            d_i;

end architecture Behavioral;
```

### VHDL stimulus process from testbench file tb_mux_2bit_4to1.vhd

```VHDL
 p_stimulus : process
    begin
        -- Report a note at the begining of stimulus process
        report "Stimulus process started" severity note;


        s_d <= "00"; s_c <= "01"; s_b <= "10"; s_a <= "11";
        s_sel <= "00"; wait for 100 ns;
        
        s_sel <= "01"; wait for 100 ns;
        
        s_sel <= "10"; wait for 100 ns;

        s_sel <= "11"; wait for 100 ns;
     
        s_d <= "11"; s_c <= "00"; s_b <= "01"; s_a <= "10";
        s_sel <= "00"; wait for 100 ns;
        
        s_sel <= "00"; wait for 100 ns;
        
        s_d <= "10"; s_c <= "01"; s_b <= "11"; s_a <= "00";
        s_sel <= "11"; wait for 100 ns;

        s_sel <= "11"; wait for 100 ns;

        -- Report a note at the end of stimulus process
        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;
```

### Screenshot with simulated time waveforms

![waveforms](Images/waveforms.PNG)

## 3. A Vivado tutorial

#### Project creation

1. V menu vybereme *File->project->new*. 

<img src="Images\01.png" alt="Založení projektu 01" style="zoom: 90%;" />

2. Otevře se nám průvodce přidání nového projektu. Tlačítkem *NEXT* pokračujeme na další stranu.

<img src="Images\02.png" alt="Založení projektu 02" style="zoom: 50%;" />

3. Vyplníme název a umístění souboru.

<img src="Images\03.png" alt="Založení projektu 03" style="zoom: 70%;" />

4. Vybereme typ projektu *RTL project*.

<img src="Images\04.png" alt="Založení projektu 04" style="zoom: 70%;" />

5. Přidáme soubor tlačítkem *Create File*.

<img src="Images\05.png" alt="Založení projektu 05" style="zoom: 70%;" />

6. Vybereme typ souboru *VHDL* a soubor pojmenujeme.

<img src="Images\06.png" alt="Založení projektu 06" style="zoom: 70%;" />

7. Pokračujem tlačítkem *NEXT*.

<img src="Images\07.png" alt="Založení projektu 07" style="zoom: 60%;" />

8. Constraints soubory přidáme do projektu později. Pokračujeme tlačítkem *NEXT*.

<img src="Images\08.png" alt="Založení projektu 08" style="zoom: 70%;" />

9. Pro přidání desky vybereme v horní části okna *Boards*. V seznamu najdeme desku *Nexys A7-50T* a potvrdíme tlačítkem *NEXT*.

<img src="Images\09.png" alt="Založení projektu 09" style="zoom: 70%;" />

10. Zkontrolujeme pomocí Project Summary a tlačítkem *Finish* potvrdíme.

<img src="Images\10.png" alt="Založení projektu 10" style="zoom: 70%;" />



#### Adding source file

1. V menu vybereme *File->Add Sources*.

   <img src="Images\11.png" alt="Přidání souboru 01" style="zoom: 100%;" />

2. Vybereme typ souboru a pokračujeme jako v bodu 5 části *Project creation*. Na konci potvrdíme tlačítkem *Finish*.

   <img src="Images\12.png" alt="Přidání souboru 02" style="zoom: 70%;" />

#### Adding testbench file

1. V menu vybereme *File->Add Sources*.

   <img src="Images\11.png" alt="Přidání souboru testbench 01" style="zoom: 100%;" />

2. Vybereme možnost *Add or create simulation sources*.

   <img src="Images\13.png" alt="Přidání souboru testbench 02" style="zoom: 60%;" />

3. Pomocí tlačítka *Create File* přidáme nový testbench soubor.

   <img src="Images\14.png" alt="Přidání souboru testbench 03" style="zoom: 70%;" />

4. Vyplníme typ a název souboru. Ideálně se začátkem názvu *"tb_"*.

   <img src="Images\15.png" alt="Přidání souboru testbench 04" style="zoom: 100%;" />

5. Dokončíme tlačítkem *Finish*.

   <img src="Images\16.png" alt="Přidání souboru testbench 05" style="zoom: 70%;" />

#### Running simulation

1. Z bočního ponelu vybereme *Run simulation* a následně z nabídky možnost *Run Behavioral Simulation*.

   <img src="Images\17.png" alt="Spuštění simulace 05" style="zoom: 80%;" />

#### Adding XDC constraints file

1. V menu vybereme *File->Add Sources*.,

   <img src="Images\11.png" alt="Přidání souboru XDC  01" style="zoom: 90%;" />

2. Vybereme možnost *Add or create constraints*.

   <img src="Images\18.png" alt="Přidání souboru XDC  02" style="zoom: 70%;" />

3. Pomocí tlačítka *Create File* přidáme nový soubor.

   <img src="Images\19.png" alt="Přidání souboru XDC  03" style="zoom: 70%;" />

4. Vybereme typ souboru *XDC* a pojmenujeme podle desky, do které chceme soubor následně nahrát.

   <img src="Images\20.png" alt="Přidání souboru XDC  04" style="zoom: 80%;" />

5. Dokončíme tlačítkem *Finish*.

   <img src="Images\21.png" alt="Přidání souboru XDC  05" style="zoom: 70%;" />