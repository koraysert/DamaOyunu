package damaOyunu.base;

import java.util.ArrayList;
import java.util.List;
import java.util.Random;
import java.util.Scanner;

public class Runner {

	// özet: dama tasları yenılemez; dolayısıyla dama tası sayısı, rakıbının tum
	// taslarından fazla olan taraf oyunu kazanır, dama tasları yukarı assaga
	// hamlede
	// yapabılır. pc nın bırden cok hamle yapabılecegı kodlara oncelık verılmıstır.

	static String giriş = "";
	static List<String> dama = new ArrayList<String>();// 8x8 tüm dama elemanlarının listesi
	static List<Integer> listxd = new ArrayList<>();// X ve @(dama) seklınde pc taslarının index listesi
	static List<Integer> listog = new ArrayList<>();// O ve G(dama) taslarının index listesi
	static List<Integer> listx = new ArrayList<>();// X pc taslarının index listesi
	static List<Integer> listo = new ArrayList<>();// O taslarının index listesi
	static List<String> harfler = new ArrayList<>();// "a","b","c","d","e","f","g","h" harf listesi

	public static void main(String[] args) {
		harfler.add("h");
		harfler.add("g");
		harfler.add("f");
		harfler.add("e");
		harfler.add("d");
		harfler.add("c");
		harfler.add("b");
		harfler.add("a");
		baslangic();
		oyun();
	}

	public static void baslangic() {
		dama.clear();
		for (int i = 0; i < 64; i++) {
			if (i < 24 && i > 7) {
				dama.add("X");
			} else if (i > 39 && i < 56) {
				dama.add("O");
			} else {
				dama.add("*");
			}

		}
		yenile();
		// for(int i=0;i<listxd.size();i++) {System.out.print(""+listxd.get(i));}
	}

	public static void yenile() {
		System.out.println("------------------------------------");
		System.out.println("    0  1  2  3  4  5  6  7");
		listxd.clear();
		listog.clear();
		listx.clear();
		listo.clear();

		for (int i = 0; i < 64; i++) {

			if (i % 8 == 0) {
				System.out.println();
				System.out.print(harfler.get(i / 8) + "   ");

			}
			if (dama.get(i).equals("O") || dama.get(i).equals("G")) {

				listog.add(i);
				if (dama.get(i).equals("O")) {
					listo.add(i);
				}

			} else if (dama.get(i).equals("X") || dama.get(i).equals("@")) {
				listxd.add(i);
				if (dama.get(i).equals("X")) {
					listx.add(i);
				}
			}

			System.out.print(dama.get(i) + "  ");

		}
		System.out.println();

	}

//0 	1 	2  	3  	4  	5  	6 	7
//8 	9 	10 	11  	12 	13 	14 	15
//16	17	18	19	20	21	22	23
//24	25	26	27	28	29	30	31
//32	33	34	35	36	37	38	39
//40	41	42	43	44	45	46	47
//48	49	50	51	52	53	54	55
//56	57	58	59	60	61	62	63

	public static void oyun() {

		giriş = girilen();

		if (giriş.equals("q")) {
			System.exit(0);
		} else {

			try {
				char[] hamle = new char[4];
				hamle = giriş.toCharArray();// girilen tas ve hamle kordınatının paremetrelerı 4 boyutlu dızıye
											// aktarılır örn:e0f0 --->'e'0'f'0'

				int tasindex = Integer.parseInt(hamle[1] + "") + (harfler.indexOf(hamle[0] + "")) * 8;
				// girilen tas kordınatının 64 elamanlı dama listinde hangı indexe karsılık
				// geldıgı bulunurak tasındexi olustururlur.

				int hamleindex = Integer.parseInt(hamle[3] + "") + (harfler.indexOf(hamle[2] + "")) * 8;
				// girilen hamle kordınatının 64 elamanlı dama listinde hangı indexe karsılık
				// geldıgı bulunurak hamleindexi olustururlur.

				if (tasindex < 56 && dama.get(tasindex).equals("G") && hamleindex == tasindex + 8
						&& dama.get(hamleindex).equals("*")) {
					dama.set(hamleindex, dama.get(tasindex));
					dama.set(tasindex, "*");
					yenile();
				} else if (tasindex > 7 && hamleindex == tasindex - 8 && dama.get(hamleindex).equals("*")) {
					dama.set(hamleindex, dama.get(tasindex));
					dama.set(tasindex, "*");
					if (hamleindex < 8) {
						dama.set(hamleindex, "G");
					}
					yenile();
				} else if (tasindex % 8 > 0 && hamleindex == tasindex - 1 && dama.get(hamleindex).equals("*")) {
					dama.set(hamleindex, dama.get(tasindex));
					dama.set(tasindex, "*");
					yenile();
				} else if (tasindex % 8 < 7 && hamleindex == tasindex + 1 && dama.get(hamleindex).equals("*")) {
					dama.set(hamleindex, dama.get(tasindex));
					dama.set(tasindex, "*");
					yenile();
				} else {
					while (!giriş.equals("p")) {// gırılen tas ve hamle kordınatı yukardakı sartları saglamıyorsa oyuncu
												// tas yıyebılecegı assadakı hamlelerı yapmak ıstemıstır ki hep son tası
												// oynamak sartı ıle tas yedıgı surece dongu devam eder ayrıca dama tası
												// yukarı asaga hamlede yapabılır ve yenilemez.

						hamle = giriş.toCharArray();
						hamleindex = Integer.parseInt(hamle[3] + "") + (harfler.indexOf(hamle[2] + "")) * 8;

						if (tasindex < 48 && dama.get(tasindex).equals("G") && hamleindex == tasindex + 16
								&& dama.get(hamleindex).equals("*") && dama.get(tasindex + 8).equals("X")) {
							dama.set(hamleindex, dama.get(tasindex));
							dama.set(tasindex + 8, "*");
							dama.set(tasindex, "*");
							tasindex = hamleindex;
							yenile();
							girilen();
						} else if (tasindex > 15 && hamleindex == tasindex - 16 && dama.get(hamleindex).equals("*")
								&& dama.get(tasindex - 8).equals("X")) {
							dama.set(hamleindex, dama.get(tasindex));
							dama.set(tasindex - 8, "*");
							dama.set(tasindex, "*");
							tasindex = hamleindex;
							if (hamleindex < 8) {
								dama.set(hamleindex, "G");
							}
							yenile();
							girilen();
						} else if (tasindex % 8 > 1 && hamleindex == tasindex - 2 && dama.get(hamleindex).equals("*")
								&& dama.get(tasindex - 1).equals("X")) {
							dama.set(hamleindex, dama.get(tasindex));
							dama.set(tasindex - 1, "*");
							dama.set(tasindex, "*");
							tasindex = hamleindex;
							yenile();
							girilen();
						} else if (tasindex % 8 < 6 && hamleindex == tasindex + 2 && dama.get(hamleindex).equals("*")
								&& dama.get(tasindex + 1).equals("X")) {
							dama.set(hamleindex, dama.get(tasindex));
							dama.set(tasindex + 1, "*");
							dama.set(tasindex, "*");
							tasindex = hamleindex;
							yenile();
							girilen();
						} else {
							System.out.println("hatalı giriş,pc oynadı");
							oyunpc();
						} // oyuncu coklu hamleler esnasında kullandıgı son tas yerıne baska bır tas
							// secerse hıle yapmış olabılecegınden oyun hakkı pc ye gecer
						if (giriş.equals("p")) {
							System.out.println("pas geçtiniz,pc oynadı");
						} // ardarda hamlelerden sonra baska hamlesı kalmadıysa gereksız kordınat gırmek
							// yerıne p ile oyuncu pas gecebılır.
					}
				}

			} catch (Exception ex) {
				if (!giriş.equals("q")) {
					System.out.println("hatalı giriş tekrar deneyin");
					oyun();
				}

			} // burada ıse oyuncu hatalı bir kordınat gırebılecegınden tekrar bır hak
				// kazanır

		}

		oyunpc();
	}

	public static void oyunpc() {
		List<Integer> taslar = new ArrayList<>();// hamle yapılamadıgında farklı tas secmek ıcın secılenlerın yer aldığı
													// liste
		int tasindex = randomx(listxd);// pc hamle için X ve @ taslarının yer aldıgı lısteden bır tas secer
		int tasindex2 = tasindex; // hamle yapılıp yapılmadıgının kontrolu ıcın ılk tasındexının tasındexı2 de
									// tutulması gerekır

		// try{
		while (true) {// oncelık coklu hamle yapabılmektır o yuzden bu kısım önce yer alır: secılen
						// tas hamle yapabıldıgı kadar oynanır,ayrıca secılen tas dama tası ıse yukarı
						// assaga
						// hamlede yapar ve yenilemez
			if (tasindex > 15 && dama.get(tasindex).equals("@") && dama.get(tasindex - 8).equals("O")
					&& dama.get(tasindex - 16).equals("*")) {
				dama.set(tasindex - 16, dama.get(tasindex));
				dama.set(tasindex - 8, "*");
				dama.set(tasindex, "*");
				tasindex = tasindex - 16;
				yenile();
			}
			if (tasindex < 48 && dama.get(tasindex + 8).equals("O") && dama.get(tasindex + 16).equals("*")) {
				dama.set(tasindex + 16, dama.get(tasindex));
				dama.set(tasindex + 8, "*");
				dama.set(tasindex, "*");
				tasindex = tasindex + 16;
				if (tasindex > 55) {
					dama.set(tasindex, "@");
				}
				yenile();
			}
			if ((tasindex) % 8 > 1 && dama.get(tasindex - 1).equals("O") && dama.get(tasindex - 2).equals("*")) {
				dama.set(tasindex - 2, dama.get(tasindex));
				dama.set(tasindex - 1, "*");
				dama.set(tasindex, "*");
				tasindex = tasindex - 2;
				yenile();
			}
			if ((tasindex) % 8 < 6 && dama.get(tasindex + 1).equals("O") && dama.get(tasindex + 2).equals("*")) {
				dama.set(tasindex + 2, dama.get(tasindex));
				dama.set(tasindex + 1, "*");
				dama.set(tasindex, "*");
				tasindex = tasindex + 2;
				yenile();

			}

			/// ---------------------------bu kısımda yukarıdakı sırada sag sol hamleler
			/// yapılamadıgında pc nın tekrar assaga yukarı gıtme ıhtımalını denemesı
			/// saglanmısıtır
			else if (tasindex > 15 && dama.get(tasindex).equals("@") && dama.get(tasindex - 8).equals("O")
					&& dama.get(tasindex - 16).equals("*")) {
				dama.set(tasindex - 16, dama.get(tasindex));
				dama.set(tasindex - 8, "*");
				dama.set(tasindex, "*");
				tasindex = tasindex - 16;
				yenile();
			} else if (tasindex < 48 && dama.get(tasindex + 8).equals("O") && dama.get(tasindex + 16).equals("*")) {
				dama.set(tasindex + 16, dama.get(tasindex));
				dama.set(tasindex + 8, "*");
				dama.set(tasindex, "*");
				tasindex = tasindex + 16;
				if (tasindex > 55) {
					dama.set(tasindex, "@");
				}
				yenile();
			}
			/// ---------------------

			else {
				if (tasindex2 == tasindex && listxd.size() != taslar.size()) {// secılen tas hamle yapamıyorsa hamle
																				// yapabılene kadar bırbırınden farklı
																				// taslar secılır hamle yapılınca
																				// tasindexi ılk basta tutulan tasindex2
																				// ile aynı olmayacagından dongu kırılır
					do {
						tasindex = randomx(listxd);
					} while (taslar.contains(tasindex));
					taslar.add(tasindex);
					tasindex2 = tasindex;
				} else {
					taslar.clear();
					break;
				}
			}
		}

		if (tasindex2 == tasindex) {// yukarda secılen taslardan hıcıbırı hamle yapamazsa yukardakı dongu kırılır ve
									// degısmeyen tasındexı sayesınde tek hamlenın yapılacagı bu kısma gecılır ve bu
									// kısımda secılen tas tek bır hamle yaparak donguyu kırar ve sıra oyuncuya
									// gecer
			while (true) {
				if (tasindex > 7 && dama.get(tasindex).equals("@") && dama.get(tasindex - 8).equals("*")) {
					dama.set(tasindex - 8, dama.get(tasindex));
					dama.set(tasindex, "*");
					tasindex = tasindex - 8;
					yenile();
					break;
				} else if (tasindex < 56 && dama.get(tasindex + 8).equals("*")) {
					dama.set(tasindex + 8, dama.get(tasindex));
					dama.set(tasindex, "*");
					tasindex = tasindex + 8;
					if (tasindex > 55) {
						dama.set(tasindex, "@");
					}
					yenile();
					break;
				} else if ((tasindex) % 8 > 0 && dama.get(tasindex - 1).equals("*")) {
					dama.set(tasindex - 1, dama.get(tasindex));
					dama.set(tasindex, "*");
					tasindex = tasindex - 1;
					yenile();
					break;
				} else if ((tasindex) % 8 < 7 && dama.get(tasindex + 1).equals("*")) {
					dama.set(tasindex + 1, dama.get(tasindex));
					dama.set(tasindex, "*");
					tasindex = tasindex + 1;
					yenile();
					break;
				} else {
					if (tasindex2 == tasindex && listxd.size() != taslar.size()) {// secılen tas hamle yapamaz ıse hamle
																					// yapılıncaya kadar farklı taslar
																					// secılır hamle yapılınca dongu
																					// kırılır ve sıra oyuncuya gecer,
																					// hıc bır tas hamle yapamaz ıse
																					// butun taslar denendıgınden dongu
																					// yıne kırılır ve sıra oyuncuya
																					// gecer
						do {
							tasindex = randomx(listxd);
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

		oyun();

		// }catch(Exception e){oyunpc();}

	}

	public static int randomx(List<Integer> listxd) {
		Random r = new Random();
		int index = r.nextInt(listxd.size());
		return listxd.get(index);
	}

	public static String girilen() {
		if ((listxd.size() - listx.size()) > listog.size()) { // pc nın damaları oyuncunun tüm taslarından fazla ıse ve
																// damalar yenılemeyecegınden oyuncunun kazanma ıhtımalı
																// kalmamıstır.giriş = "q"; ile oyun sonlandırılır
			System.out.println("kazanma ihtimaliniz kalmadı");
			giriş = "q";
		} else if ((listog.size() - listo.size()) > listxd.size()) { // oyuncunun damaları pc nıntüm taslarından fazla
																		// ıse ve damalar yenılemeyecegınden pc nın
																		// kazanma ıhtımalı kalmamıstır.giriş = "q"; ile
																		// oyun sonl
			System.out.println("kazandınız");
			giriş = "q";
		} else {
			System.out.println("tas ve hamle kordinatı giriniz örn: e0f0 gibi; kaybedilen tas sayisi: pc: "
					+ (16 - listxd.size()) + " oyuncu: " + (16 - listog.size()));

			Scanner sc = new Scanner(System.in);
			giriş = sc.next();
		}
		return giriş;
	}

}
