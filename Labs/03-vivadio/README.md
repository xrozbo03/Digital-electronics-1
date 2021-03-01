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