**Ruby REPL** solution. The `pbpaste` command must be available in the `$PATH`, and should return the contents in the clipboard (macOS has this command by default).

1. **Inverse Captcha**

    ```ruby
    -> x { (x.chars + [x[0]]).each_cons(2).select {|a,b|a==b}.map{|a,b|a.to_i}.reduce(0, &:+) } [`pbpaste`.strip]
    ```
    
    ```ruby
    -> x { a = x.chars; a.zip(a.rotate(a.length / 2)).select {|a,b|a==b}.map{|a,b|a.to_i}.reduce(0, &:+) }[`pbpaste`.strip]
    ```

2. **Corruption Checksum**

    ```ruby
    -> x { x.lines.map { |l| -l.split.map(&:to_i).minmax.reduce(&:-) }.reduce(&:+) }[`pbpaste`]
    ```
    
    ```ruby
    -> x { x.lines.map { |l| l.split.map(&:to_i).permutation(2).find { |a, b| a % b == 0 }.reduce(&:/) }.reduce(&:+) }[`pbpaste`]
    ```

3. **Spiral Memory**

    ```ruby
    -> x { c = Math.sqrt(x - 1).floor; i = (x - (c ** 2 + 1)) % (c + 1); ((c + 1) / 2) + ((c - 1) / 2 - i).abs }[`pbpaste`.to_i]
    ```

4. **High-Entropy Passphrases**

    ```ruby
    -> x { x.lines.map(&:split).count { |w| w.uniq.length == w.length } }[`pbpaste`]
    ```

    ```ruby
    -> x { x.lines.map { |l| l.split.map(&:chars).map(&:sort) }.count { |w| w.uniq == w } }[`pbpaste`]
    ```
5. **A Maze of Twisty Trampolines, All Alike**

    ```ruby
    -> a { c = n = 0; while (0...a.length).include?(c); c += (a[c] += 1) - 1; n += 1; end; n }[`pbpaste`.split.map(&:to_i)]
    ```
    
    ```ruby
    -> a { c = n = 0; while (0...a.length).include?(c); c += a[i = c]; a[i] += (a[i] >= 3 ? -1 : 1); n += 1; end; n }[`pbpaste`.split.map(&:to_i)]
    ```
6. **Memory Reallocation**

    ```ruby
    nx = -> x { max = x.max; m = x.index(max); n = x.dup; n[m] = 0; (m + 1...m + 1 + max).map { |i| i % x.length }.each { |i| n[i] += 1 }; n }
    -> a { seen = { }; c = 0; while !seen[a]; c += 1; seen[a] = true; a = nx[a]; end; c }[`pbpaste`.split.map(&:to_i)]
    ```

    ```ruby
    nx = -> x { max = x.max; m = x.index(max); n = x.dup; n[m] = 0; (m + 1...m + 1 + max).map { |i| i % x.length }.each { |i| n[i] += 1 }; n }
    -> a { seen = { }; c = 0; while !seen[a]; c += 1; seen[a] = c; a = nx[a]; end; c - seen[a] + 1 }[`pbpaste`.split.map(&:to_i)]
    ```

7. **Recursive Circus**

    ```ruby
    -> x { s = {}; p = {}; x.scan(/(\w+).*?->\s*(.+)/).each { |a, b| b.strip.split(', ').each{|n|p[n]=a}; s[a]=1 }; s.keys.select{|z|!p[z]} }[`pbpaste`]
    ```

    ```ruby
    s = {}; p = {}; c = {}; `pbpaste`.scan(/(\w+) \((\d+)\)(?: -> )?(.*)/).each { |a, w, b| b.length > 0 && b.strip.split(', ').each{|n|p[n]=a; (c[a]||=[]) << n}; s[a]=w.to_i }
    wei = -> n { s[n] + (c[n] || []).map(&wei).reduce(0, &:+) }
    
    # s['name'] -> Weight of the program
    # p['name'] -> Name of the program’s parent (or nil)
    # c['name'] -> List of the program’s children
    # wei['name'] -> Weight of the program, including all its children.
    
    # Use this to find the nodes with unbalanced child weight
    c.select { |n, ch| ch.map(&wei).uniq.length > 1 }
    
    # The rest can be figured out manually using the REPL
    ```
