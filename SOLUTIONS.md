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
