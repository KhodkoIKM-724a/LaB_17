#include <iostream>
#include <locale>
#include <stdexcept>
#include <cmath>

using namespace std;

class Kimnata {
protected:
    double dovzhyna;
    double shyryna;
    double vysota;
    int kilkistVikon;

public:
    Kimnata(double d, double s, double v, int vikna) {
        if (d <= 0 || s <= 0 || v <= 0 || vikna < 0)
            throw invalid_argument("Недопустимі значення параметрів кімнати.");
        dovzhyna = d;
        shyryna = s;
        vysota = v;
        kilkistVikon = vikna;
    }

    double Ploshcha() const {
        return dovzhyna * shyryna;
    }

    double Obem() const {
        return dovzhyna * shyryna * vysota;
    }

    void Stan() const {
        cout << "Довжина: " << dovzhyna << " м\n";
        cout << "Ширина: " << shyryna << " м\n";
        cout << "Висота стелі: " << vysota << " м\n";
        cout << "Кількість вікон: " << kilkistVikon << "\n";
        cout << "Площа: " << Ploshcha() << " м²\n";
        cout << "Об'єм: " << Obem() << " м³\n";
    }
};

class KimnataZRemontom : public Kimnata {
public:
    KimnataZRemontom(double d, double s, double v, int vikna)
        : Kimnata(d, s, v, vikna) {
    }

    double RozrakhunokShpaler(double shyrynaShpalery, double dovzhynaShpalery, double visotaShpalery) {
        double ploshchaStin = 2 * (dovzhyna + shyryna) * vysota;
        double ploshchaOdniyeiShpalery = shyrynaShpalery * visotaShpalery;
        if (ploshchaOdniyeiShpalery <= 0)
            throw invalid_argument("Недопустимі розміри шпалер.");
        return ceil(ploshchaStin / ploshchaOdniyeiShpalery);
    }
};

int main() {
    setlocale(LC_ALL, "");
    system("chcp 65001 > nul");

    try {
        KimnataZRemontom kimnata(5.0, 4.0, 2.5, 2);
        kimnata.Stan();
        double shyrynaShpalery = 0.53;
        double dovzhynaShpalery = 10.0;
        double visotaShpalery = 2.5;
        double kilkistRuloniv = kimnata.RozrakhunokShpaler(shyrynaShpalery, dovzhynaShpalery, visotaShpalery);
        cout << "Потрібна кількість рулонів шпалер: " << kilkistRuloniv << endl;
    }
    catch (const exception& ex) {
        cerr << "Помилка: " << ex.what() << endl;
    }
}
