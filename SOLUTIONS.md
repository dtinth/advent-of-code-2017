**Ruby REPL** (irb) solution. The `pbpaste` command must be available in the `$PATH`, and should return the contents in the clipboard (macOS has this command by default).

1. **Inverse Captcha**

    ```ruby
    # Part 1
    -> x { (x.chars + [x[0]]).each_cons(2).select {|a,b|a==b}.map{|a,b|a.to_i}.reduce(0, &:+) } [`pbpaste`.strip]
    ```

    ```ruby
    # Part 2
    -> x { a = x.chars; a.zip(a.rotate(a.length / 2)).select {|a,b|a==b}.map{|a,b|a.to_i}.reduce(0, &:+) }[`pbpaste`.strip]
    ```

2. **Corruption Checksum**

    ```ruby
    # Part 1
    -> x { x.lines.map { |l| -l.split.map(&:to_i).minmax.reduce(&:-) }.reduce(&:+) }[`pbpaste`]
    ```

    ```ruby
    # Part 2
    -> x { x.lines.map { |l| l.split.map(&:to_i).permutation(2).find { |a, b| a % b == 0 }.reduce(&:/) }.reduce(&:+) }[`pbpaste`]
    ```

3. **Spiral Memory**

    ```ruby
    # Part 1
    -> x { c = Math.sqrt(x - 1).floor; i = (x - (c ** 2 + 1)) % (c + 1); ((c + 1) / 2) + ((c - 1) / 2 - i).abs }[`pbpaste`.to_i]

    # Part 1, but rewritten with more straightforward logic to prepare for part 2.
    -> n { x = y = 0; dx, dy = 1, 0; (2..n).each { x += dx; y += dy; dx, dy = 0, 1 if x > 0 && y == -x + 1; dx, dy = -1, 0 if x > 0 && y == x; dx, dy = 0, -1 if x < 0 && y == -x; dx, dy = 1, 0 if x < 0 && y == x; }; [x, y] }[`pbpaste`.to_i]
    ```

    ```ruby
    # Part 2
    -> n { v = Hash.new(0); v[[0, 0]] = 1; x = y = 0; dx, dy = 1, 0; loop { x += dx; y += dy; dx, dy = 0, 1 if x > 0 && y == -x + 1; dx, dy = -1, 0 if x > 0 && y == x; dx, dy = 0, -1 if x < 0 && y == -x; dx, dy = 1, 0 if x < 0 && y == x; v[[x, y]] = (-1..1).map { |i| (-1..1).map { |j| v[[x+i,y+j]] } }.flatten.reduce(0, &:+); return v[[x, y]] if v[[x, y]] > n } }[`pbpaste`.to_i]
    ```

4. **High-Entropy Passphrases**

    ```ruby
    # Part 1
    -> x { x.lines.map(&:split).count { |w| w.uniq.length == w.length } }[`pbpaste`]
    ```

    ```ruby
    # Part 2
    -> x { x.lines.map { |l| l.split.map(&:chars).map(&:sort) }.count { |w| w.uniq == w } }[`pbpaste`]
    ```
5. **A Maze of Twisty Trampolines, All Alike**

    ```ruby
    # Part 1
    -> a { c = n = 0; while (0...a.length).include?(c); c += (a[c] += 1) - 1; n += 1; end; n }[`pbpaste`.split.map(&:to_i)]
    ```

    ```ruby
    # Part 2
    -> a { c = n = 0; while (0...a.length).include?(c); c += a[i = c]; a[i] += (a[i] >= 3 ? -1 : 1); n += 1; end; n }[`pbpaste`.split.map(&:to_i)]
    ```
6. **Memory Reallocation** (31st, 38th)

    ```ruby
    # Part 1
    nx = -> x { max = x.max; m = x.index(max); n = x.dup; n[m] = 0; (m + 1...m + 1 + max).map { |i| i % x.length }.each { |i| n[i] += 1 }; n }
    -> a { seen = { }; c = 0; while !seen[a]; c += 1; seen[a] = true; a = nx[a]; end; c }[`pbpaste`.split.map(&:to_i)]
    ```

    ```ruby
    # Part 2
    nx = -> x { max = x.max; m = x.index(max); n = x.dup; n[m] = 0; (m + 1...m + 1 + max).map { |i| i % x.length }.each { |i| n[i] += 1 }; n }
    -> a { seen = { }; c = 0; while !seen[a]; c += 1; seen[a] = c; a = nx[a]; end; c - seen[a] + 1 }[`pbpaste`.split.map(&:to_i)]
    ```

7. **Recursive Circus** (47th, 71st)

    ```ruby
    # Part 1
    -> x { s = {}; p = {}; x.scan(/(\w+).*?->\s*(.+)/).each { |a, b| b.strip.split(', ').each{|n|p[n]=a}; s[a]=1 }; s.keys.select{|z|!p[z]} }[`pbpaste`]
    ```

    ```ruby
    # Part 2
    s = {}; p = {}; c = {}; `pbpaste`.scan(/(\w+) \((\d+)\)(?: -> )?(.*)/).each { |a, w, b| b.length > 0 && b.strip.split(', ').each{|n|p[n]=a; (c[a]||=[]) << n}; s[a]=w.to_i }
    # s['name'] -> Weight of the program
    # p['name'] -> Name of the program’s parent (or nil)
    # c['name'] -> List of the program’s children

    wei = -> n { s[n] + (c[n] || []).map(&wei).reduce(0, &:+) }
    # wei['name'] -> Weight of the program, including all its children.
    
    # Use this to find the nodes with unbalanced child weight
    c.select { |n, ch| ch.map(&wei).uniq.length > 1 }
    
    # The final answer can be found by manually inspecting weights in the REPL. Here’s mine:
    # 2.4.1 :085 > wei['nieyygi']
    #  => 11781
    # 2.4.1 :086 > wei['mxgpbvf']
    #  => 1117
    # 2.4.1 :087 > wei['cfqdsb']
    #  => 1117
    # 2.4.1 :088 > wei['yfejqb']
    #  => 1117
    # 2.4.1 :089 > wei['ptshtrn']
    #  => 1122
    # 2.4.1 :090 > s['ptshtrn']
    #  => 526
    # 2.4.1 :091 > 526 - (1117 - 1122)
    #  => 521
    ```
