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
