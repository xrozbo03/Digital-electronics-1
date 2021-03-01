# Lab 3: Introduction to Vivado

## 1. Preparation

| **Dec. equivalent** | **B[1:0]** | **A[1:0]** | **B is greater than A** |
| 0 | 0 0 | 0 0 | 0 |
| 1 | 0 0 | 0 1 | 0 |
| 2 | 0 0 | 1 0 | 0 |
| 3 | 0 0 | 1 1 | 0 |
| 4 | 0 1 | 0 0 | 1 |


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