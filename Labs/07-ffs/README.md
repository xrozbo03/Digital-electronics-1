# Lab 7: Latches and Flip-flops

## 1. Preparation

### Characteristic equations and completed tables for D, JK, T flip-flops

![equations](Images/equations.png)

| **clk** | **D** | **qn** | **q(n+1)** | **Comments** |
| :-: | :-: | :-: | :-: | :-- |
| ![rising](Images/eq_uparrow.png) | 0 | 0 | 0 | No change |
| ![rising](Images/eq_uparrow.png) | 0 | 1 | 0 | RESET |
| ![rising](Images/eq_uparrow.png) | 1 | 0 | 1 | No change    |
| ![rising](Images/eq_uparrow.png) |   1   | 1 | 1 | SET          |

| **clk** | **J** | **K** | **qn** | **q(n+1)** | **Comments** |
| :-: | :-: | :-: | :-: | :-: | :-- |
| ![rising](Images/eq_uparrow.png) | 0 | 0 | 0 | 0 | No change |
| ![rising](Images/eq_uparrow.png) | 0 | 0 | 1 | 1 | No change |
| ![rising](Images/eq_uparrow.png) | 0 | 1 | 0 | 0 | RESET |
| ![rising](Images/eq_uparrow.png) | 0 | 1 | 1 | 0 | RESET |
| ![rising](Images/eq_uparrow.png) | 1 | 0 | 0 | 1 | SET |
| ![rising](Images/eq_uparrow.png) | 1 | 0 | 1 | 1 | SET |
| ![rising](Images/eq_uparrow.png) | 1 | 1 | 0 | 1 | Toggle |
| ![rising](Images/eq_uparrow.png) | 1 | 1 | 1 | 0 | Toggle |

| **clk** | **T** | **qn** | **q(n+1)** | **Comments** |
| :-: | :-: | :-: | :-: | :-- |
| ![rising](Images/eq_uparrow.png) | 0 | 0 | 0 | No change |
| ![rising](Images/eq_uparrow.png) | 0 | 1 | 1 | No change |
| ![rising](Images/eq_uparrow.png) | 1 | 0 | 1 | Invert (Toggle) |
| ![rising](Images/eq_uparrow.png) | 1 | 1 | 0 | Invert (Toggle) |

## 2. D latch.

### VHDL code listing of the process `p_d_latch`

```vhdl
	p_d_latch : process (d, arst, en)
	begin
   		if (arst = '1') then
        	q     <= '0';
        	q_bar <= '1';
    	elsif (en = '1') then
        	q     <= d;
        	q_bar <= not d;
    	end if;   
	end process p_d_latch;
```

### Listing of VHDL reset and stimulus processes from the testbench `tb_d_latch`

```vhdl
--------------------------------------------------------------------
-- Reset generation process
--------------------------------------------------------------------
 	p_reset_gen : process
    begin
        s_arst <= '0';
        wait for 40 ns;
        
        s_arst <= '1';
        wait for 53 ns;
        
        s_arst <= '0';
        wait for 260 ns;
        
        s_arst <= '1';
        wait;
    end process p_reset_gen;
    
--------------------------------------------------------------------
-- Data generation process
--------------------------------------------------------------------
     p_stimulus : process
     begin
        report "Stimulus process started" severity note;
        s_d   <= '0';
        s_en  <= '0';
        
        -- d sekv
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        wait for 10 ns;
        s_d   <= '1';
        
        wait for 5 ns;
        -- Expected output
        assert ((s_q = '0') and (s_q_bar = '1'))
        -- If false, then report an error
        report "Test failed for input on 55ns" severity error;
        
        wait for 5 ns;
        s_d   <= '0';
        --/d sekv
        
        s_en   <= '1';
        
        -- d sekv
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        --/d sekv
        
        -- d sekv
        wait for 10 ns;
        s_d   <= '1';
        
        wait for 5 ns;
         -- Expected output
        assert ((s_q = '1') and (s_q_bar = '0'))
        -- If false, then report an error
        report "Test failed for input on 135ns" severity error;
        
        wait for 5 ns;
        s_d   <= '0';
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        --/d sekv
        
        s_en  <= '0';
        
        -- d sekv
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        --/d sekv
        
        wait for 35 ns;
        -- Expected output
        assert ((s_q = '1') and (s_q_bar = '0'))
        -- If false, then report an error
        report "Test failed for input on 275ns" severity error;
        
        wait for 100 ns;
        -- Expected output
        assert ((s_q = '0') and (s_q_bar = '1'))
        -- If false, then report an error
        report "Test failed for input on 375ns" severity error;
        
        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;
```

### Screenshot with simulated time waveforms

![waveforms](Images/waveforms2.png)

## 3. Flip-flops

### `p_d_ff_arst`

#### VHDL code listing of the processes

```vhdl
    p_d_ff_arst : process (clk, arst)
    begin
        if (arst = '1') then
            q     <= '0';
            q_bar <= '1';
        elsif rising_edge(clk) then
            q     <= d;
            q_bar <= not d;
        end if;
        
    end process p_d_ff_arst;
```

#### Listing of VHDL clock, reset and stimulus processes from the testbench

```vhdl
    --------------------------------------------------------------------
    -- Clock generation process
    --------------------------------------------------------------------
    p_clk_gen : process
    begin
        while now < 750 ns loop         -- 75 periods of 100MHz clock
            s_clk_100MHz <= '0';
            wait for c_CLK_100MHZ_PERIOD / 2;
            s_clk_100MHz <= '1';
            wait for c_CLK_100MHZ_PERIOD / 2;
        end loop;
        wait;
    end process p_clk_gen;
    
    --------------------------------------------------------------------
    -- Reset generation process
    --------------------------------------------------------------------
    p_reset_gen : process
    begin
        s_arst <= '0';
        wait for 53 ns;
        
        s_arst <= '1';
        wait for 53 ns;
        
        s_arst <= '0';
        wait for 53 ns;
        
        s_arst <= '1';
        wait;
    end process p_reset_gen;

    --------------------------------------------------------------------
    -- Data generation process
    --------------------------------------------------------------------
     p_stimulus : process
     begin
        report "Stimulus process started" severity note;
        
        -- d sekv
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        wait for 10 ns;
        s_d   <= '1';
        
        wait for 2 ns;
        -- Expected output
        assert ((s_q = '0') and (s_q_bar = '1'))
        -- If false, then report an error
        report "Test failed for input on 32ns" severity error;
        
        wait for 5 ns;
        -- Expected output
        assert ((s_q = '1') and (s_q_bar = '0'))
        -- If false, then report an error
        report "Test failed for input on 37ns" severity error;
        
        wait for 3 ns;
        s_d   <= '0';
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        --/d sekv
        
        wait for 3 ns;
        
        -- d sekv
        wait for 10 ns;
        s_d   <= '1';
        
        assert ((s_q = '0') and (s_q_bar = '1'))
        -- If false, then report an error
        report "Test failed for input on 73ns" severity error;
        
        wait for 10 ns;
        s_d   <= '0';
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';     
        wait for 10 ns;
        s_d   <= '1';
        
        assert ((s_q = '0') and (s_q_bar = '1'))
        -- If false, then report an error
        report "Test failed for input on 113ns" severity error;
        
        wait for 10 ns;
        s_d   <= '0';
        --/d sekv
        
        -- d sekv
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        --/d sekv
        
        -- d sekv
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        --/d sekv
                
        report "Stimulus process finished" severity note;
        wait;
     end process p_stimulus;
```

#### Screenshot, with simulated time waveforms

![waveforms](Images/waveforms3_1.png)

### `p_d_ff_rst`

#### VHDL code listing of the processes

```vhdl
    p_d_ff_rst : process (clk)
    begin
        if rising_edge(clk) then
            if (rst = '1') then
                q     <= '0';
                q_bar <= '1';
            else
                q     <= d;
                q_bar <= not d;
            end if;
        end if;
        
    end process p_d_ff_rst;
```

#### Listing of VHDL clock, reset and stimulus processes from the testbench

```vhdl
    --------------------------------------------------------------------
    -- Clock generation process
    --------------------------------------------------------------------
    p_clk_gen : process
    begin
        while now < 750 ns loop         -- 75 periods of 100MHz clock
            s_clk_100MHz <= '0';
            wait for c_CLK_100MHZ_PERIOD / 2;
            s_clk_100MHz <= '1';
            wait for c_CLK_100MHZ_PERIOD / 2;
        end loop;
        wait;
    end process p_clk_gen;
    
    --------------------------------------------------------------------
    -- Reset generation process
    --------------------------------------------------------------------
    p_reset_gen : process
    begin
        s_rst <= '0';
        wait for 53 ns;
        
        s_rst <= '1';
        wait for 53 ns;
        
        s_rst <= '0';
        wait for 53 ns;
        
        s_rst <= '1';
        wait;
    end process p_reset_gen;
    
    --------------------------------------------------------------------
    -- Data generation process
    --------------------------------------------------------------------
     p_stimulus : process
     begin
        report "Stimulus process started" severity note;
        
         -- d sekv
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        wait for 10 ns;
        s_d   <= '1';
        
        wait for 2 ns;
        -- Expected output
        assert ((s_q = '0') and (s_q_bar = '1'))
        -- If false, then report an error
        report "Test failed for input on 32ns" severity error;
        
        wait for 5 ns;
        -- Expected output
        assert ((s_q = '1') and (s_q_bar = '0'))
        -- If false, then report an error
        report "Test failed for input on 37ns" severity error;
        
        wait for 3 ns;
        s_d   <= '0';
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        --/d sekv
        
        wait for 3 ns;
        
        -- d sekv
        wait for 10 ns;
        s_d   <= '1';
        
        assert ((s_q = '0') and (s_q_bar = '1'))
        -- If false, then report an error
        report "Test failed for input on 73ns" severity error;
        
        wait for 10 ns;
        s_d   <= '0';
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';     
        wait for 10 ns;
        s_d   <= '1';      
        wait for 10 ns;
        s_d   <= '0';
        --/d sekv
        
        -- d sekv
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
               
        assert ((s_q = '1') and (s_q_bar = '0'))
        -- If false, then report an error
        report "Test failed for input on 163ns" severity error;
        
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        --/d sekv
        
        -- d sekv
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        wait for 10 ns;
        s_d   <= '1';
        wait for 10 ns;
        s_d   <= '0';
        --/d sekv
                
        report "Stimulus process finished" severity note;
        wait;
     end process p_stimulus;
```

#### Screenshot, with simulated time waveforms

![waveforms](Images/waveforms3_2.png)

### `p_jk_ff_rst`

#### VHDL code listing of the processes

```vhdl
    p_jk_ff_rst : process (clk)
    begin
        if rising_edge(clk) then
            if (rst = '1') then
                s_q <= '0';
            else    
                if (j = '0' and k = '0') then
                    s_q <= s_q;
                    
                elsif (j = '0' and k = '1') then
                    s_q <= '0';
                    
                elsif (j = '1' and k = '0') then
                    s_q <= '1';
                
                elsif (j = '1' and k = '1') then
                    s_q <= not s_q;
                    
                end if;
            end if;
        end if;
        
    end process p_jk_ff_rst;
    
    q       <= s_q;
    q_bar   <= not s_q;
```

#### Listing of VHDL clock, reset and stimulus processes from the testbench

```vhdl
    --------------------------------------------------------------------
    -- Clock generation process
    --------------------------------------------------------------------
    p_clk_gen : process
    begin
        while now < 750 ns loop         -- 75 periods of 100MHz clock
            s_clk_100MHz <= '0';
            wait for c_CLK_100MHZ_PERIOD / 2;
            s_clk_100MHz <= '1';
            wait for c_CLK_100MHZ_PERIOD / 2;
        end loop;
        wait;
    end process p_clk_gen;
    
    --------------------------------------------------------------------
    -- Reset generation process
    --------------------------------------------------------------------
    p_reset_gen : process
    begin
        s_rst <= '0';
        wait for 53 ns;
        
        s_rst <= '1';
        wait for 15 ns;
        
        s_rst <= '0';

        wait;
    end process p_reset_gen;
    
    --------------------------------------------------------------------
    -- Data generation process
    --------------------------------------------------------------------
     p_stimulus : process
     begin
        report "Stimulus process started" severity note;
       
        wait for 13 ns;
        s_j <= '0';
        s_k <= '0';
        wait for 20 ns;
        s_j <= '0';
        s_k <= '1';
        
        wait for 5 ns;
        assert ((s_q = '0') and (s_q_bar = '1'))
        -- If false, then report an error
        report "Test failed for input on 38ns" severity error;
        
        wait for 15 ns;
        s_j <= '1';
        s_k <= '0';
        
        wait for 10 ns;
        assert ((s_q = '0') and (s_q_bar = '1'))
        -- If false, then report an error
        report "Test failed for input on 63ns" severity error;
        
        wait for 10 ns;
        s_j <= '1';
        s_k <= '1';
        
        assert ((s_q = '0') and (s_q_bar = '1'))
        -- If false, then report an error
        report "Test failed for input on 73ns" severity error;
        
        wait for 10 ns;
        assert ((s_q = '1') and (s_q_bar = '0'))
        -- If false, then report an error
        report "Test failed for input on 83ns" severity error;
        
        wait for 10 ns;
        s_j <= '0';
        s_k <= '0';
        wait for 20 ns;
        s_j <= '0';
        s_k <= '1';
        wait for 20 ns;
        s_j <= '1';
        s_k <= '0';
        wait for 20 ns;
        s_j <= '1';
        s_k <= '1';
        
        wait for 20 ns;
        s_j <= '0';
        s_k <= '0';
        wait for 20 ns;
        s_j <= '0';
        s_k <= '1';
        wait for 20 ns;
        s_j <= '1';
        s_k <= '0';
        wait for 20 ns;
        s_j <= '1';
        s_k <= '1';
        
        wait for 55 ns;
        assert ((s_q = '1') and (s_q_bar = '0'))
        -- If false, then report an error
        report "Test failed for input on 288ns" severity error;
                       
        report "Stimulus process finished" severity note;
        wait;
     end process p_stimulus;
```

#### Screenshot, with simulated time waveforms

![waveforms](Images/waveforms3_3.png)

### `p_t_ff_rst`

#### VHDL code listing of the processes

```vhdl
    p_t_ff_rst : process (clk)
    begin
        if rising_edge(clk) then
            if (rst = '1') then
                s_q <= '0';
            elsif (t = '1') then
                s_q <= not s_q;
            elsif (t = '0') then
                s_q <= s_q;
            end if;
        end if;
        
    end process p_t_ff_rst;
    
    q       <= s_q;
    q_bar   <= not s_q;
```

#### Listing of VHDL clock, reset and stimulus processes from the testbench

```vhdl
    --------------------------------------------------------------------
    -- Clock generation process
    --------------------------------------------------------------------
    p_clk_gen : process
    begin
        while now < 750 ns loop         -- 75 periods of 100MHz clock
            s_clk_100MHz <= '0';
            wait for c_CLK_100MHZ_PERIOD / 2;
            s_clk_100MHz <= '1';
            wait for c_CLK_100MHZ_PERIOD / 2;
        end loop;
        wait;
    end process p_clk_gen;

    --------------------------------------------------------------------
    -- Reset generation process
    --------------------------------------------------------------------
    p_reset_gen : process
    begin
        s_rst <= '0';
        wait for 53 ns;
        
        s_rst <= '1';
        wait for 53 ns;
        
        s_rst <= '0';
        wait;
    end process p_reset_gen;
  
    --------------------------------------------------------------------
    -- Data generation process
    --------------------------------------------------------------------
     p_stimulus : process
     begin
        report "Stimulus process started" severity note;
    
        -- t sekv
        wait for 10 ns;
        s_t   <= '1';
        wait for 10 ns;
        s_t   <= '0';
        wait for 10 ns;
        s_t   <= '1';
        wait for 10 ns;
        s_t   <= '0';
        wait for 10 ns;
        s_t   <= '1';
        wait for 10 ns;
        s_t   <= '0';
        -- t sekv
         
        -- Expected output
        assert ((s_q = '0') and (s_q_bar = '1'))
        -- If false, then report an error
        report "Test failed for input on 60ns" severity error; 
         
        -- t sekv
        wait for 10 ns;
        s_t   <= '1';
        wait for 10 ns;
        s_t   <= '0';
        wait for 10 ns;
        s_t   <= '1';
        wait for 10 ns;
        s_t   <= '0';
        wait for 10 ns;
        s_t   <= '1';
        wait for 10 ns;
        s_t   <= '0';
        -- t sekv
         
        -- Expected output
        assert ((s_q = '1') and (s_q_bar = '0'))
        -- If false, then report an error
        report "Test failed for input on 120ns" severity error;
        
        wait for 40 ns;
        -- Expected output
        assert ((s_q = '1') and (s_q_bar = '0'))
        -- If false, then report an error
        report "Test failed for input on 160ns" severity error;
                
        wait for 40 ns;
        s_t   <= '1';
        
        wait for 30 ns;
        -- Expected output
        assert ((s_q = '0') and (s_q_bar = '1'))
        -- If false, then report an error
        report "Test failed for input on 230ns" severity error;
        
        wait for 50 ns;
        s_t   <= '0';

                   
        report "Stimulus process finished" severity note;
        wait;
     end process p_stimulus;
```

#### Screenshot, with simulated time waveforms

![waveforms](Images/waveforms3_4.png)

## 4. Shift register

### Image of the shift register schematic

![Shift register](Images/schematic.png)