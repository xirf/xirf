```rs
use std::{io::Write, thread::sleep, time::Duration};
const W:usize=64;const H:usize=24;
fn main(){
    let mut a=vec![0u8;W*H];let mut b=a.clone();let mut x=88172645463393265u64;
    for i in 0..a.len(){x^=x<<7;x^=x>>9;a[i]=(x as u8)&1;}
    loop{
        print!("\x1b[2J\x1b[H");
        for y in 0..H{for z in 0..W{print!("{}",if a[y*W+z]==1{'#'}else{' '});}println!();}
        std::io::stdout().flush().ok();
        for y in 0..H{for z in 0..W{
            let mut n=0;
            for dy in[-1isize,0,1]{for dx in[-1isize,0,1]{
                if dx!=0||dy!=0{
                    let xx=((z as isize+dx+W as isize)%W as isize)as usize;
                    let yy=((y as isize+dy+H as isize)%H as isize)as usize;
                    n+=a[yy*W+xx] as u8;
                }
            }}
            b[y*W+z]=((n==3)||(n==2&&a[y*W+z]==1))as u8;
        }}
        std::mem::swap(&mut a,&mut b);
        sleep(Duration::from_millis(60));
    }
}
```
