"# SystemStressTest" 


import java.util.ArrayList;
import java.util.List;

public class SystemStressTest {
    private static final List<Object> memory = new ArrayList<>();


    public static void main(String[] args) {
        stressCPU();

        new Thread(SystemStressTest::stressRAM).start();
    }

    //cpu
    public static void stressCPU() {
        int cores = Runtime.getRuntime().availableProcessors();
        System.out.println("CPU magok száma: " + cores);

        for (int i = 0; i < cores; i++) {
            new Thread(() -> {
                while (true) {
                    double value = 0;
                    for (int j = 0; j < 1_000_000; j++) {
                        value += Math.sqrt(j) * Math.random();
                    }
                }
            }).start();
        }
    }


    //ram
    public static void stressRAM() {
        while (true) {
            byte[] block = new byte[50 * 1024 * 1024];
            memory.add(block);

            for (int i = 0; i < block.length; i += 4096) {
                block[i] = 1;
            }
        }
    }
}
