# DamaOyunu

package damaOyunu.base;

import java.util.ArrayList;
import java.util.List;
import java.util.Random;
import java.util.Scanner;

public class Runner {

	static String giriş = "";
	static List<String> dama = new ArrayList<String>();
	static List<Integer> listx = new ArrayList<>();
	static List<Integer> listxx = new ArrayList<>();

	public static void main(String[] args) {

		baslangic();
		oyun();
	}

	public static void oyun() {

		giriş = girilen();
		if (giriş.equals("q")) {
			System.exit(0);
		} else {

			try {

				int tasindex = dama.indexOf(giriş.split("-")[0]);
				int hamleindex = Integer.parseInt(giriş.split("-")[1]);

				if (tasindex > 7 && hamleindex == tasindex - 8 && dama.get(hamleindex).equals("*")) {
					dama.set(hamleindex, hamleindex + "");
					dama.set(tasindex, "*");
					yenile();
				} else if (tasindex % 8 != 0 && hamleindex == tasindex - 1 && dama.get(hamleindex).equals("*")) {
					dama.set(hamleindex, hamleindex + "");
					dama.set(tasindex, "*");
					yenile();
				} else if (tasindex % 8 != 7 && hamleindex == tasindex + 1 && dama.get(hamleindex).equals("*")) {
					dama.set(hamleindex, hamleindex + "");
					dama.set(tasindex, "*");
					yenile();
				} else {
					while (true) {

						hamleindex = Integer.parseInt(giriş.split("-")[1]);

						if (tasindex > 15 && hamleindex == tasindex - 16 && dama.get(hamleindex).equals("*")
								&& dama.get(tasindex - 8).equals("X")) {
							dama.set(hamleindex, hamleindex + "");
							dama.set(tasindex - 8, "*");
							dama.set(tasindex, "*");
							tasindex = hamleindex;
							yenile();
							girilen();
						} else if (tasindex % 8 != 1 && hamleindex == tasindex - 2 && dama.get(hamleindex).equals("*")
								&& dama.get(tasindex - 1).equals("X")) {
							dama.set(hamleindex, hamleindex + "");
							dama.set(tasindex - 1, "*");
							dama.set(tasindex, "*");
							tasindex = hamleindex;
							yenile();
							girilen();
						} else if (tasindex % 8 != 6 && hamleindex == tasindex + 2 && dama.get(hamleindex).equals("*")
								&& dama.get(tasindex + 1).equals("X")) {
							dama.set(hamleindex, hamleindex + "");
							dama.set(tasindex + 1, "*");
							dama.set(tasindex, "*");
							tasindex = hamleindex;
							yenile();
							girilen();
						} else {
							System.out.println("hatalı giriş,pc oynadı");
							oyunpc();
						}
					}
				}

			} catch (Exception ex) {
				System.out.println("hatalı giriş tekrar deneyin");
				oyun();
			}
		}

		oyunpc();
	}

	public static void oyunpc() {
		List<Integer> taslar = new ArrayList<>();
		int tasindex = randomx(listx);
		int tasindex2 = tasindex;

		// try{
		while (true) {
			if (tasindex < 47 && dama.get(tasindex + 8).matches("[0-9]+") && dama.get(tasindex + 16).equals("*")) {
				dama.set(tasindex + 16, "X");
				dama.set(tasindex + 8, "*");
				dama.set(tasindex, "*");
				tasindex = tasindex + 16;
				yenile();
			} else if (tasindex % 8 != 1 && dama.get(tasindex - 1).matches("[0-9]+")
					&& dama.get(tasindex - 2).equals("*")) {
				dama.set(tasindex - 2, "X");
				dama.set(tasindex - 1, "*");
				dama.set(tasindex, "*");
				tasindex = tasindex - 2;
				yenile();
			} else if (tasindex % 8 != 6 && dama.get(tasindex + 1).matches("[0-9]+")
					&& dama.get(tasindex + 2).equals("*")) {
				dama.set(tasindex + 2, "X");
				dama.set(tasindex + 1, "*");
				dama.set(tasindex, "*");
				tasindex = tasindex + 2;
				yenile();
			} else {
				if (tasindex2 == tasindex && listx.size() != taslar.size()) {
					do {
						tasindex = randomx(listx);
					} while (taslar.contains(tasindex));
					taslar.add(tasindex);
					tasindex2 = tasindex;
				} else {
					taslar.clear();
					break;
				}
			}

			if (tasindex2 == tasindex) {
				while (true) {
					if (tasindex < 56 && dama.get(tasindex + 8).equals("*")) {
						dama.set(tasindex + 8, "X");
						dama.set(tasindex, "*");
						tasindex = tasindex + 8;
						yenile();
						break;
					} else if (tasindex % 8 != 0 && dama.get(tasindex - 1).equals("*")) {
						dama.set(tasindex - 1, "X");
						dama.set(tasindex, "*");
						tasindex = tasindex - 1;
						yenile();
						break;
					} else if (tasindex % 8 != 7 && dama.get(tasindex + 1).equals("*")) {
						dama.set(tasindex + 1, "X");
						dama.set(tasindex, "*");
						tasindex = tasindex + 1;
						yenile();
						break;
					} else {
						if (tasindex2 == tasindex && listx.size() != taslar.size()) {
							do {
								tasindex = randomx(listx);
							} while (taslar.contains(tasindex));
							taslar.add(tasindex);
							tasindex2 = tasindex;
						} else {
							taslar.clear();
							break;
						}
					}
				}
			}

//			if(tasindex2==tasindex) { 
//				 if(tasindex <  56 && dama.get(tasindex+8).equals("*")) {dama.set(tasindex+8,"X");dama.set(tasindex,"*"); yenile();}
//			else if(tasindex %8!=0 && dama.get(tasindex-1).equals("*")) {dama.set(tasindex-1,"X");dama.set(tasindex,"*"); yenile();}
//			else if(tasindex %8!=7 && dama.get(tasindex+1).equals("*")) {dama.set(tasindex+1,"X");dama.set(tasindex,"*"); yenile();}} else {System.out.print("pc pas");}    

		}

		// }catch(Exception e){oyunpc();}
		oyun();
	}

	public static void baslangic() {
		dama.clear();
		for (int i = 0; i < 64; i++) {
			if (i < 24 && i > 7) {
				dama.add("X");
			} else if (i > 39 && i < 56) {
				dama.add(i + "");
			} else {
				dama.add("*");
			}

		}
		yenile();
		// for(int i=0;i<listx.size();i++) {System.out.print(""+listx.get(i));}
	}

	public static void yenile() {
		System.out.println("--------------------------------");
		listx.clear();
		listxx.clear();
		for (int i = 0; i < 64; i++) {

			if (i % 8 == 0 && i != 0) {
				System.out.println();
			}
			if (dama.get(i).matches("[0-9]+")) {
				if (dama.get(i).length() == 2) {
					System.out.print(dama.get(i) + " ");
				} else {
					System.out.print(dama.get(i) + "  ");
				}
			} else {
				if (dama.get(i).equals("X")) {
					listx.add(i);
				} else if (dama.get(i).equals("*")) {
					listxx.add(i);
				}

				System.out.print(dama.get(i) + "  ");
			}

		}
		System.out.println();

	}

	// 0 1 2 3 4 5 6 7
	// 8 9 10 11 12 13 14 15
	// 16 17 18 19 20 21 22 23
	// 24 25 26 27 28 29 30 31
	// 32 33 34 35 36 37 38 39
	// 40 41 42 43 44 45 46 47
	// 48 49 50 51 52 53 54 55
	// 56 57 58 59 60 61 62 63

	public static int randomx(List<Integer> listx) {
		Random r = new Random();
		int index = r.nextInt(listx.size());
		return listx.get(index);
	}

//public static int randomxx(List<Integer>listxx) {Random r = new Random();int index = r.nextInt(listxx.size());return listxx.get(index);}

	public static String girilen() {
		System.out.println("tas numarası-hamle numarası giriniz.kaybedilen tas sayisi: pc: " + (16 - listx.size())
				+ " oyuncu: " + (16 - (64 - (listx.size() + listxx.size()))));
		Scanner sc = new Scanner(System.in);
		giriş = sc.next();
		return giriş;
	}

}
