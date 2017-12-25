Please read the repository description.

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

8. **I Heard You Like Registers** (13th, 26th)

    I heard you like eval.

    ```ruby
    # Part 1
    -> x { d = Hash.new(0); eval x.gsub(/(\w+) (inc|dec) (-?\d+) if (\w+) (\S+) (\S+)/) { "d['#{$1}'] #{$2 == 'inc' ? '+=' : '-='} #{$3} if d['#{$4}'] #{$5} #{$6}" }; d.values.max }[`pbpaste`]
    ```

    ```ruby
    # Part 2
    -> x { d = Hash.new(0); m = 0; eval x.gsub(/(\w+) (inc|dec) (-?\d+) if (\w+) (\S+) (\S+)/) { "d['#{$1}'] #{$2 == 'inc' ? '+=' : '-='} #{$3} if d['#{$4}'] #{$5} #{$6}; m = [m, d.values.max || 0].max" }; m }[`pbpaste`]
    ```

9. **Stream Processing** (16th, 15th)

    ```ruby
    -> x { g = false; n = 0; t = 0; s = false; x.chars.each { |c| if s; s = false; elsif g && c == '>'; g = false; elsif g && c == '!'; s = true; elsif g; elsif c == '<'; g = true; elsif c == '{'; n += 1; elsif c == '}'; t += n; n -= 1; end }; t }[`pbpaste`]
    ```

    ```ruby
    -> x { g = false; n = 0; t = 0; gc = 0; s = false; x.chars.each { |c| if s; s = false; elsif g && c == '>'; g = false; elsif g && c == '!'; s = true; elsif g; gc += 1; elsif c == '<'; g = true; elsif c == '{'; n += 1; elsif c == '}'; t += n; n -= 1; end }; gc }[`pbpaste`]
    ```

10. **Knot Hash**

    ```ruby
    # Part 1
    -> n, a { d = (0...n).to_a; r = 0; skip = 0; a.each { |c| d[0...c] = d[0...c].reverse; d = d.rotate(c + skip); r += c + skip; skip += 1; p d.rotate(n - (r % n)) }; r = d.rotate(n - (r % n)); r[0] * r[1] }[256, `pbpaste`.split(',').map(&:to_i)]
    ```

    ```ruby
    # Part 2
    -> n, a { d = (0...n).to_a; r = 0; skip = 0; 64.times { a.each { |c| d[0...c] = d[0...c].reverse; d = d.rotate(c + skip); r += c + skip; skip += 1; }; }; r = d.rotate(n - (r % n)); r.each_slice(16).map { |s| "%02x" % s.reduce(&:^) }.join }[256, [*`pbpaste`.strip.bytes, 17, 31, 73, 47, 23]]
    ```

11. **Hex Ed**

    ```ruby
    # Part 1
    -> a { x = y = 0; a.each { |c| if c == 'ne'; y += 1; x += 1; elsif c == 'se'; y -= 1; x += 1; elsif c == 'nw'; y += 1; x -= 1; elsif c == 'sw'; y -= 1; x -= 1; elsif c == 's'; y -= 2; elsif c == 'n'; y += 2; end }; x.abs + [(y.abs - x.abs) / 2, 0].max }[ `pbpaste`.strip.split(',') ]
    ```

    ```ruby
    # Part 2
    -> a { x = y = 0; a.map { |c| if c == 'ne'; y += 1; x += 1; elsif c == 'se'; y -= 1; x += 1; elsif c == 'nw'; y += 1; x -= 1; elsif c == 'sw'; y -= 1; x -= 1; elsif c == 's'; y -= 2; elsif c == 'n'; y += 2; end; x.abs + [(y.abs - x.abs) / 2, 0].max }.max }[ `pbpaste`.strip.split(',') ]
    ```

    Made lots of mistake today and didn’t get into the leaderboard.
    So, for fun, I’d just try to make the solution “single-statement,” i.e.
    not using any statement terminator (newlines or `;`)..

    ```ruby
    # Part 1
    -> a { -> ((x, y)) { x.abs + [0, (y.abs - x.abs) / 2].max }[a.map { |c| { 'ne' => [1, 1], 'nw' => [-1, 1], 'se' => [1, -1], 'sw' => [-1, -1], 's' => [0, -2], 'n' => [0, 2] }[c] }.transpose.map { |v| v.reduce(&:+) }] }[ `pbpaste`.strip.split(',') ]
    ```

    ```ruby
    # Part 2
    -> a { a.map { |c| { 'ne' => [1, 1], 'nw' => [-1, 1], 'se' => [1, -1], 'sw' => [-1, -1], 's' => [0, -2], 'n' => [0, 2] }[c] }.reduce ([[0, 0]]) { |a, (x, y)| a << [a.last[0] + x, a.last[1] + y] } }[ `pbpaste`.strip.split(',') ].map { |x, y| x.abs + [0, (y.abs - x.abs) / 2].max }.max
    ```

12. **Digital Plumber**

    ```ruby
    # Load input into REPL session
    data = -> h { h.keys { |k| h[k].dup.each { |z| h[z] << k } }; h }[Hash[`pbpaste`.lines.map { |l| a, b = l.split('<->').map(&:strip); [a.to_i, b.split(', ').map(&:to_i)] }]]
    ```

    ```ruby
    # Part 1
    visited = { }; g = -> x { return if visited[x]; visited[x] = true; data[x].each(&g) }; g[0]; visited.keys.length
    ```

    ```ruby
    # Part 2
    visited = { }; g = -> x { return if visited[x]; visited[x] = true; data[x].each(&g) }; (0..1999).count { |z| f = !visited[z]; g[z]; f }
    ```

13. **Packet Scanners**

    ```ruby
    # Load input into REPL session
    f = Hash[`pbpaste`.scan(/(\d+): (\d+)/).map { |x| x.map(&:to_i) }]
    ```

    ```ruby
    # Part 1
    d = Hash[f.map { |a, c| [a, [c, 0, 1]] }]; (0..f.keys.max).map { |c| hit = d[c] && d[c][1] == 0; d.each { |k, v| d[k] = s[v] }; hit ? c * d[c][0] : 0 }.reduce(&:+)
    ```

    ```ruby
    # Part 2
    pos = -> a, t { g = t % (a * 2 - 2); [g, (a * 2 - 2) - g].min }
    (0..4000000000).find { |d| v = f.keys.count { |k| pos[f[k], k + d] == 0 }; p [d, v] if d % 10000 == 0; v == 0 }
    ```

14. **Disk Defragmentation**

    ```ruby
    # Loading data into REPL session
    kh = -> tx { -> n, a { d = (0...n).to_a; r = 0; skip = 0; 64.times { a.each { |c| d[0...c] = d[0...c].reverse; d = d.rotate(c + skip); r += c + skip; skip += 1; }; }; r = d.rotate(n - (r % n)); r.each_slice(16).map { |s| "%02x" % s.reduce(&:^) }.join }[256, [*tx.strip.bytes, 17, 31, 73, 47, 23]] }
    bits = (0...128).map { |i| kh["flqrgnkx-#{i}"].to_i(16).to_s(2).rjust(128, '0') }
    ```

    ```ruby
    # Part 1
    bits.join.count('1')
    ```

    ```ruby
    # Part 2
    visit = Hash.new(false); v = -> i, j { return if i < 0 || i >= 128 || j < 0 || j >= 128 || visit[[i, j]] || bits[i][j] != '1'; visit[[i, j]] = true; v[i + 1, j]; v[i - 1, j]; v[i, j + 1]; v[i, j - 1] }; cn = 0; (0...128).each { |i| (0...128).each { |j| unless visit[[i, j]] || bits[i][j] != '1'; v[i, j]; cn += 1; end } }; cn
    ```

15. **Dueling Generators**

    ```ruby
    # Part 1
    a, b = 65, 8921; z = 0; 40000000.times { |i| p [i, z] if i % 500000 == 0; a *= 16807; b *= 48271; a %= 2147483647; b %= 2147483647; if (a & 0xFFFF) == (b & 0xFFFF); z += 1; p i; end }; z
    ```

    ```ruby
    # Part 2
    a, b = 65, 8921; z = 0; 5000000.times { |i| p [i, z] if i % 100000 == 0; loop { a *= 16807; a %= 2147483647; break if a % 4 == 0 }; loop { b *= 48271; b %= 2147483647; break if b % 8 == 0 }; if (a & 0xFFFF) == (b & 0xFFFF); z += 1; p i; end }; z
    ```

16. **Permutation Promenade**

    ```ruby
    # Part 1
    -> d, a { a.each { |x| if x =~ /^s(\d+)/; d.rotate!(-$1.to_i); elsif x =~ /^x(\d+)\/(\d+)/; d[$1.to_i], d[$2.to_i]=  d[$2.to_i], d[$1.to_i] ; elsif x =~ /^p(.)\/(.)/; ia = d.index($1); ib = d.index($2); d[ia], d[ib]=  d[ib], d[ia] end; p d.join }; d.join } ['abcdefghijklmnop'.chars, `pbpaste`.strip.split(',')]
    ```

17. **Spinlock**

    ```ruby
    # Part 1
    -> v { a = [0]; x = 0; (1..2017).each { |c| x += v; x %= a.length; x += 1; a[x, 0] = [c]; }; a[(x + 1) % a.length] }[3]
    ```

    I couldn’t think of a [more efficient solution](https://www.reddit.com/r/adventofcode/comments/7kc0xw/2017_day_17_solutions/drd5yek/?utm_content=permalink&utm_medium=front&utm_source=reddit&utm_name=adventofcode), so I brute-forced this in C.

    ```c
    #include <stdio.h>
    struct N {
      struct N* next;
      int val;
    };
    struct N heap[50000001];
    int main () {
      struct N first;
      struct N *cur = &first;
      int vv = 0;
      first.val = 0;
      first.next = &first;
      int i, j;
      for (i = 1; i <= 50000000; i ++) {
        if (i % 10000 == 0) { printf("[%d]\n", i); }
        for (j = 0; j < 3; j ++) cur = cur->next;
        struct N *v = &heap[vv++];
        v->val = i;
        v->next = cur->next;
        cur->next = v;
        cur = v;
      }
      struct N *it = &first;
      printf("after first %d\n", first.next->val);
      return 0;
    }
    ```

18. **Duet**

    Code for part 1 was lost :sob: but it’s written in Ruby.

    For part 2, I ended up at rank 114, because I wasted some time trying to implement CSP in Ruby myself. I shouldn’t have done it since implementing something like this is a limited time is very bug prone for me and I got into an infinite loop.

    ```js
    // Part 2
    const csp = require('js-csp')
    const code = INPUT.split(/\n/).map(a => a.trim().split(/\s+/))

    function * prog (p, inbox, outbox) {
      const d = { p }
      const get = k => k.match(/\d/) ? +k : (d[k] || 0)
      let i
      let sent = 0
      const report = () => {
        console.log({ i, d }, code[i])
      }
      for (i = 0; (report(), i < code.length); i++) {
        const c = code[i]
        if (c[0] === 'snd') {
          sent += 1
          const val = get(c[1])
          console.log('Program', p, 'sent', val, 'from', c[1], 'total', sent, 'time(s)')
          yield csp.put(outbox, val)
          continue
        }
        if (c[0] === 'rcv') {
          const  val = yield csp.take(inbox)
          d[c[1]] = val
          console.log(p, 'recv', val, c[1])
          continue
        }
        if (c[0] === 'set') {
          d[c[1]] = get(c[2])
          continue
        }
        if (c[0] === 'add') {
          d[c[1]] += get(c[2])
          continue
        }
        if (c[0] === 'mul') {
          d[c[1]] *= get(c[2])
          continue
        }
        if (c[0] === 'mod') {
          d[c[1]] %= get(c[2])
          continue
        }
        if (c[0] === 'jgz') {
          if (get(c[1]) > 0) {
            i -= 1
            i += get(c[2])
          }
          continue
        }
      }
    }

    const m0 = csp.chan(99999999)
    const m1 = csp.chan(99999999)
    csp.go(function * () { yield * prog(0, m0, m1) })
    csp.go(function * () { yield * prog(1, m1, m0) })
    ```

19. **A Series of Tubes**

    ```ruby
    # Both part 1 and part 2
    IN = `pbpaste`

    maze = {}
    start = nil
    IN.lines.each_with_index { |v, i|
      v.chars.each_with_index { |c, j|
        if c =~ /[A-Z]/
          if i == 0
            start = [i, j]
          end
          maze[[i, j]] = c
        elsif c =~ /\S/
          if i == 0
            start = [i, j]
          end
          maze[[i, j]] = '!'
        end
      }
    }

    direction = [0, 1]

    cur = start.dup
    rl = -> d { [d[1], -d[0]] }
    rr = -> d { [-d[1], d[0]] }
    nx = -> c, d { [c[0] + d[0], c[1] + d[1]] }
    nn = 0
    loop {
      break if !maze[cur]
      # p cur
      nn += 1
      print maze[cur] if maze[cur] != '!'
      if !maze[nx[cur, direction]]
        if maze[nx[cur, rl[direction]]]
          direction = rl[direction]
        elsif maze[nx[cur, rr[direction]]]
          direction = rr[direction]
        end
      end
      cur = nx[cur, direction]
    }
    puts
    puts nn
    ```

20. **Particle Swarm**

    I messed up in the second part, because I didn’t adjust the velocity _before_ adjusting the position. While this doesn’t have any effect in part 1, in part 2, it causes the particles to not collide at all.

    ```ruby
    # Data loading
    particles = `pbpaste`.lines.map { |l| l.scan(/-?\d+/).map(&:to_i).each_slice(3).to_a }

    # Calculating the particle state
    nx = -> ps { ps.map { |(x,y,z),(vx,vy,vz),(ax,ay,az)| vx+=ax;vy+=ay;vz+=az;[[x+vx,y+vy,z+vz],[vx,vy,vz],[ax,ay,az]] } }

    # Start simulation
    c = particles

    # Part 1 (keep running this until answer stops changing)
    500.times { c = nx[c] }; c.map { |(x,y,z),v,a| (x.abs+y.abs+z.abs).abs }.each_with_index.min

    # Part 2 (keep running this until answer stops changing)
    500.times { c = nx[c]; d = Hash.new(0); c.each { |s,v,a| d[s] += 1 }; c.reject! { |s,v,a| d[s] > 1 } }; c.length
    ```

21. **Fractal Art**

    ```ruby
    # Loading the rulebook
    IN = `pbpaste`
    rules = {}; IN.scan(/(\S+) => (\S+)/).map { |a, b| [a.split('/'), b.split('/')] }.each { |a, b| rules[a] = b }; rules

    # Initial state and algorithm for flipping and rotating.
    data = ['.#.', '..#', '###']
    flip = -> m { m.reverse }
    flip2 = -> m { m.map(&:reverse) }
    flip3 = -> m { m.reverse.map(&:reverse) }
    flip4 = -> m { m.map(&:chars).transpose.map(&:join) }
    flip5 = -> m { m.map(&:chars).transpose.map(&:join).reverse }
    flip6 = -> m { m.map(&:chars).transpose.map(&:join).map(&:reverse) }
    flip7 = -> m { m.map(&:chars).transpose.map(&:join).map(&:reverse).reverse }

    # Logic to enhance the pattern
    nx = -> m { pz = m.length.even? ? 2 : 3; l = m.length / pz; (0...l).map { |i| (0...l).map { |j| inp = (0...pz).map { |k| (0...pz).map { |l| m[k+i*pz][l+j*pz] }.join }; rules[inp] || rules[flip[inp]] || rules[flip2[inp]] || rules[flip3[inp]] || rules[flip4[inp]] || rules[flip5[inp]] || rules[flip6[inp]] || rules[flip7[inp]] }.transpose.map(&:join) }.flatten(1) }

    # Part 1
    puts nx[nx[nx[nx[nx[data]]]]].join.count('#')

    # Part 2
    puts nx[nx[nx[nx[nx[nx[nx[nx[nx[nx[nx[nx[nx[nx[nx[nx[nx[nx[data]]]]]]]]]]]]]]]]]].join.count('#')
    ```

22. **Sporifica Virus**

    ```ruby
    # Loading the map (also setting the initial position to the centre)
    input = `pbpaste`; initial_map = {}; input.lines.each_with_index { |l, i| l.strip.chars.each_with_index { |c, j| initial_map[[i,j]] = '#' if c == '#' } }; initial_pos = [input.strip.lines.length / 2, input.lines.first.strip.length / 2]

    # Utility functions for turning left/right
    rl = -> d { [-d[1], d[0]] }
    rr = -> d { [d[1], -d[0]] }

    # Initialize/reset simulation state
    map = initial_map.dup; pos = initial_pos.dup; direction = [-1, 0]; infect = 0

    # Part 1
    10000.times { if map[pos]; direction = rr[direction]; map.delete(pos); else; direction = rl[direction]; map[pos] = '#'; infect += 1; end; pos = [pos[0] + direction[0], pos[1] + direction[1]] }; infect

    # Part 2 (inline logic refactored into procs for easier editing)
    handle_clean = -> { direction = rl[direction]; map[pos] = 'W' }
    handle_weakened = -> { map[pos] = '#'; infect += 1 }
    handle_infected = -> { direction = rr[direction]; map[pos] = 'F' }
    handle_flagged = -> { direction = rr[rr[direction]]; map.delete(pos) }
    10000000.times { |nn| puts nn if nn % 100000 == 0; (case map[pos]; when '#'; handle_infected; when 'F'; handle_flagged; when 'W'; handle_weakened; else; handle_clean; end)[]; pos = [pos[0] + direction[0], pos[1] + direction[1]] }; infect
    ```

23. **Coprocessor Conflagration**

    First part, I just copied the JS code and make a few modifications!

    Second part, haven’t figured that out yet. :(

24. **Electromagnetic Moat**

    ```ruby
    # Loading input
    input = `pbpaste`
    connectors = input.lines.each_with_index.map { |l, i| [i, *l.strip.split('/').map(&:to_i)] }
    nodes = {}; connect = -> f, t, id { node = (nodes[f] ||= []); node << [t, id] }; connectors.each { |i, j, k| connect[j, k, i]; connect[k, j, i] if k != j }; nodes

    # Solving part 1
    visited = {}; path = []; sum = 0; max = 0; traverse = -> c { p [[*path, c], sum]; max = sum if sum > max; node = nodes[c]; return unless node; node.each { |t, i| unless visited[i]; val = connectors[i][1] + connectors[i][2]; sum += val; path << i; visited[i] = true; traverse[t]; path.pop; visited[i] = false; sum -= val; end; } }; traverse[0]; max
    ```

    Part 2 didn’t finish yet.

25. **The Halting Problem**

    ```ruby
    # First, I manually converted the input into a state transition
    # table manually.
    transition = {
      :a => { 0 => [ 1, :right, :b ], 1 => [ 0, :left, :b ] },
      :b => { 0 => [ 1, :left, :a ], 1 => [ 1, :right, :a ] },
    }

    # This runs the turing machine
    state = :a; tape = Hash.new(0); pointer = 0
    6.times { |i|
      instruction = transition[state][tape[pointer]]
      tape[pointer] = instruction[0]
      pointer += instruction[1] == :left ? -1 : 1
      state = instruction[2]
    }

    # Part 1: This computes the checksum
    tape.values.count(1)
    ```

    Don’t have enough stars for part 2 yet...
