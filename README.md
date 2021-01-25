
import java.text.SimpleDateFormat;
import java.util.Date;

/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */

/**
 *
 * @author DELL
 */
public class pt {
    public static void main(String[] args) {
        long sss = 24*60*60*25567L;
        short[] arr = {225,72,40,195,105,242,38,249,225,72,40,208,41,242,12,33,225,72,40,208,41,242,52,101};
        for (int pos = 0; pos < 3; pos++) {
            double time = 0.0;
            int ind = pos*8;
            for (int i = ind; i < ind+8; i++) {
                time = time + arr[i] * Math.pow(2, (ind + 3 - i)*8);
            }
            String result = new SimpleDateFormat("YYYY:MM:dd HH:mm:ss.SSS").format(new Date((long)(1000*(time-sss)))) ;
            System.out.println(result);
        }
    }
}

