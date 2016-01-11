title: Java模拟两小球的碰撞
date: 2016-01-12 01:09:39
tags:
- java

catgories:
- 杂

---

###### 2016年第二更...

最近写了一个模拟两个小球,无能量损失碰撞的java小程序....

##### [介绍 java 绘图的网站](https://www3.ntu.edu.sg/home/ehchua/programming/java/J4b_CustomGraphics.html)

##### 模拟两个小球碰撞  [网上的demo](http://www.cnblogs.com/kenkofox/archive/2011/09/06/2168944.html)

##### [碰撞时速度的变化](http://tina0152.blog.163.com/blog/static/119447958200910229109326/)

*碰撞的代码*:

```java
public class Collision {

    public double base = 50;
    public double w = 500;
    private double eps = 1e-5;

    public double distanceSQ(Circle c1,Circle c2) {

        double dx = Math.abs(c1.x - c2.x);
        double dy = Math.abs(c1.y - c2.y);

        return dx*dx+dy*dy;
    }

    public void CirToCirS(Circle c , Vector<Circle> vc) {

        boolean flag=false;
        for(Circle othercircle : vc) {

            if(othercircle.equals(c)) {
                flag=true;
                continue;
            }
            if(flag==false) {
                continue;
            }
            if(flag==true) {
                CircleToCircle(c,othercircle);
            }
        }
    }

    public void CircleToCircle(Circle c1,Circle c2) {

        if(CheckCirlceToCircle(c1,c2)) {
            /// change vector

            VVector vss = new VVector(c2.x-c1.x,c2.y-c1.y);
            vss = vss.unitfy();
            VVector vst = new VVector(vss.y,-vss.x);

            VVector v1 = new VVector(c1.vx,c1.vy);
            VVector v2 = new VVector(c2.vx,c2.vy);

            double v1s = v1.dotMul(vss);
            double v2s = v2.dotMul(vss);
            double v1t = v1.dotMul(vst);
            double v2t = v2.dotMul(vst);

            double temp = v1s; v1s=v2s; v2s=temp;

            VVector v1tV = vst.mul(v1t);
            VVector v1sV = vss.mul(v1s);
            VVector v2tV = vst.mul(v2t);
            VVector v2sV = vss.mul(v2s);

            c1.vx=v1tV.x+v1sV.x; c1.vy=v1tV.y+v1sV.y;
            c2.vx=v2tV.x+v2sV.x; c2.vy=v2tV.y+v2sV.y;

        }
    }

    public void CirlcleToWall(Circle cirlce) {
        Vector<Integer> vi = new Vector<Integer>();
        vi = CheckCircleToWall(cirlce);

        for(Integer x : vi) {
            if(x==1||x==3) {
               cirlce.vx*=-1;
            }
            else if(x==2||x==4) {
               cirlce.vy*=-1;
            }
        }
    }

    public boolean CheckCirlceToCircle(Circle c1,Circle c2) {

        if(distanceSQ(c1, c2)+eps<(double)((c1.r+c2.r)*(c1.r+c2.r)))
            return true;
        else return false;
    }

    /// 0 no Collison
    /// 1 North 2 East 3 South 4 West
    public Vector<Integer> CheckCircleToWall(Circle c) {

        Vector<Integer> ret = new Vector<Integer>();

        if(c.x+c.r>w+base) {
            ret.add(1);
        }
        if(c.x-c.r<base) {
            ret.add(3);
        }

        if(c.y+c.r>w+base) {
            ret.add(4);
        }
        if(c.y-c.r<base) {
            ret.add(2);
        }

        return ret;
    }
}
```