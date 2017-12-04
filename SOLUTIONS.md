1. **Inverse Captcha**

    ```ruby
    -> x { (x.chars + [x[0]]).each_cons(2).select {|a,b|a==b}.map{|a,b|a.to_i}.reduce(0, &:+) } [`pbpaste`.strip]
    ```
    
    ```ruby
    -> x { a = x.chars; a.zip(a.rotate(a.length / 2)).select {|a,b|a==b}.map{|a,b|a.to_i}.reduce(0, &:+) }[`pbpaste`.strip]
    ```
