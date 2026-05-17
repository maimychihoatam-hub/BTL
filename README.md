#include <iostream>
#include <vector>
#include <iomanip>
#include <limits>
#include <stdexcept>

using namespace std;

void line(int n){
    cout << setfill('-') << setw(n) << "-" << setfill(' ') << endl;
}

int nhapInt(string text){
    int x;
    while(true){
        cout << text;
        cin >> x;
        if(cin.fail()){
            cout << "Nhap sai! Vui long nhap so!\n";
            cin.clear();
            cin.ignore(
                numeric_limits<streamsize>::max(),
                '\n'
            );
        }else return x;
    }
}
string nhapString(string text){
    string s;
    cout << text;
    getline(cin >> ws, s);
    return s;
}

class ConNguoi{
protected:
    string ten,sdt;
public:
    virtual void nhap() = 0;
    virtual void xuat() const = 0;
    string getTen() const { return ten; }
    virtual ~ConNguoi() {}
};
class KhachHang : public ConNguoi {
private:
    int diem;
    vector<string> lichSuChoi;
public:
    KhachHang(){
        diem = 0;
    }
    void nhap() override {
        ten = nhapString("Nhap ten khach hang: ");
        sdt = nhapString("Nhap so dien thoai: ");
    }
    void congDiem(double tien){
        diem += tien / 10000;
    }
    int getDiem() const{
        return diem;
    }
    string xepHang() const{
        if(diem >= 100)
            return "Gold";

        if(diem >= 50)
            return "Silver";

        return "Thuong";
    }
    double layGiamGia() const{
        if(xepHang() == "Gold")
            return 0.10;
        if(xepHang() == "Silver")
            return 0.05;
        return 0;
    }
    void themLichSu(string ls){
        lichSuChoi.push_back(ls);
    }
    void hienThiLichSu() const{
        cout << "\n===== LICH SU CHOI =====\n";
        if(lichSuChoi.empty()){
            cout << "Chua co lich su!\n";
            return;
        }
        for(string x : lichSuChoi){
            cout << "- " << x << endl;
        }
    }
    void xuat() const override{
        cout << "| "
             << left  << setw(15) << ten
             << "| "
             << left  << setw(12) << sdt
             << "| "
             << right << setw(8)  << diem
             << " | "
             << left  << setw(10) << xepHang()
             << " |\n";
    }
};
class NhanVien : public ConNguoi{
private:
    double luong;
    string chucVu;
public:
    void nhap() override{
        ten = nhapString("Nhap ten NV: ");
        sdt = nhapString("Nhap SDT NV: ");
        chucVu = nhapString("Nhap chuc vu: ");
        luong = nhapInt("Nhap luong: ");
    }
    void xuat() const override{
        cout << "| "
             << left  << setw(15) << ten
             << "| "
             << left  << setw(12) << sdt
             << "| "
             << left  << setw(12) << chucVu
             << "| "
             << right << setw(10)
             << fixed << setprecision(0)
             << luong
             << " |\n";
    }
};
class DoUong{
private:
    int ma;
    string ten;
    double gia;
public:
    void setDU(int m,string t,double g){
        ma = m;
        ten = t;
        gia = g;
    }
    int getMa() const { return ma; }
    string getTen() const { 
        return ten; 
    }
    double getGia() const { 
        return gia; 
    }
    void xuat() const{
        cout << "| "
             << left  << setw(5)  << ma
             << "| "
             << left  << setw(20) << ten
             << "| "
             << right << setw(10)
             << fixed << setprecision(0)
             << gia
             << " |\n";
    }
};

class BanBida{
private:
    int ma;
    string loai;
    double gia;
    bool trangThai;
    string tenKhach;
    int hMo,pMo;
    int hDong,pDong;
    vector<pair<string,double>> dsNuoc;
public:
    BanBida(){
        trangThai = false;
        hMo = 0;
        pMo = 0;
    }
    bool getTrangThai() const{
        return trangThai;
    }
    void setBan(int m,string l,double g){
        ma = m;
        loai = l;
        gia = g;
    }

    int getMa() const{ return ma; }
    int getHMo() const{ return hMo; }
    int getPMo() const{ return pMo; }
    int getHDong()const{ return hDong; }
    int getPDong()const{ return pDong; }
    string getLoai()const{ return loai; }
    string getTenKhach() const{
        return tenKhach;
    }
    vector<pair<string,double>> getDSNuoc() const{
        return dsNuoc;
    }
    double getGia() const{
        return gia;
    }

    void xuat() const{
        cout << "| "
             << left  << setw(5)  << ma
             << "| "
             << left  << setw(15) << loai
             << "| "
             << right << setw(10)
             << fixed << setprecision(0)
             << gia
             << " | "
             << left  << setw(18) << tenKhach
             << "| "
             << left  << setw(12)
             << (trangThai ? "Dang choi" : "Con trong")
             << " |\n";
    }
    void xuatChiTiet(){
        line(50);
        cout << "Ma ban      : " << ma << endl;
        cout << "Loai ban    : " << loai << endl;
        cout << "Gia / gio   : " << gia << endl;
        if(trangThai){
            cout << "Khach       : "
                 << tenKhach << endl;
            cout << "Trang thai  : Dang choi\n";
            cout << "Gio mo      : ";
            if(hMo < 10) cout << "0";
            cout << hMo << ":";
            if(pMo < 10) cout << "0";
            cout << pMo << endl;
            cout << "\nDo uong da goi:\n";
            if(dsNuoc.empty()){
                cout << "Khong co\n";
            }else{
                for(auto x : dsNuoc){
                    cout << "- "
                         << x.first
                         << " : "
                         << x.second
                         << " VND\n";
                }
            }
        }else{
            cout << "Trang thai  : Con trong\n";
        }
        line(50);
    }

    void moBan(string ten,int h,int p){
        if(trangThai)
            throw runtime_error("Ban dang su dung!");
        tenKhach = ten;
        hMo = h;
        pMo = p;
        trangThai = true;
    }
    
    void themDoUong(string ten,double tien){
        dsNuoc.push_back({ten,tien});
    }

    double dongBan(int h,int p){
        if(!trangThai)
            throw runtime_error("Ban chua mo!");
        hDong = h;
        pDong = p;
        int mo =
            hMo * 60 + pMo;
        int dong =
            hDong * 60 + pDong;
        int tongPhut =
            dong - mo;
        if(tongPhut <= 0)
            tongPhut = 60;
        double soGio =
            tongPhut / 60.0;
        double tong =
            soGio * gia;
        for(auto x : dsNuoc)
            tong += x.second;
        dsNuoc.clear();
        trangThai = false;
        tenKhach = "";
        return tong;
    }
};

class HoaDon{
private:
    int maHD,maBan;
    int hMo,pMo;
    int hDong,pDong;
    double tongTien;
public:
    HoaDon(int mhd,
           int mb,
           int hm,
           int pm,
           int hd,
           int pd,
           double tong){
        maHD = mhd;
        maBan = mb;
        hMo = hm;
        pMo = pm;
        hDong = hd;
        pDong = pd;
        tongTien = tong;
    }
    double getTongTien() const{
        return tongTien;
    }
    void xuat() const{
        string tg;
        if(hMo < 10) tg += "0";
        tg += to_string(hMo) + ":";
        if(pMo < 10) tg += "0";
        tg += to_string(pMo);
        tg += " - ";
        if(hDong < 10) tg += "0";
        tg += to_string(hDong) + ":";
        if(pDong < 10) tg += "0";
        tg += to_string(pDong);
        cout << "| "
             << left  << setw(5)  << maHD
             << "| "
             << left  << setw(5)  << maBan
             << "| "
             << left  << setw(13) << tg
             << "| "
             << right << setw(12)
             << fixed << setprecision(0)
             << tongTien
             << " |\n";
    }
};

template<class T>
class DanhSach{
private:
    vector<T> ds;
public:
    void them(T x){
        ds.push_back(x);
    }
    vector<T>& get(){
        return ds;
    }
};

class QuanLy{
private:
    DanhSach<BanBida> dsBan;
    DanhSach<DoUong> dsDU;
    DanhSach<HoaDon> dsHD;
    vector<ConNguoi*> dsNguoi;
public:
    QuanLy(){
        khoiTaoBan();
        khoiTaoDU();
    }

    void khoiTaoBan(){

        int ma = 1;
    
        // 3 ban Libre
        for(int i=0;i<3;i++){
            BanBida b;
            b.setBan(ma++,"Libre",50000);
            dsBan.them(b);
        }
    
        // 4 ban Pool
        for(int i=0;i<4;i++){
            BanBida b;
            b.setBan(ma++,"Pool",60000);
            dsBan.them(b);
        }
    
        // 2 ban Snooker
        for(int i=0;i<2;i++){
            BanBida b;
            b.setBan(ma++,"Snooker",70000);
            dsBan.them(b);
        }
    }

    void khoiTaoDU(){
        DoUong d1,d2,d3,d4;
        d1.setDU(1,"Coca",15000);
        d2.setDU(2,"Sting",15000);
        d3.setDU(3,"Bo Hut",20000);
        d4.setDU(4,"Nuoc Suoi",10000);
        
        dsDU.them(d1);
        dsDU.them(d2);
        dsDU.them(d3);
        dsDU.them(d4);
        
    }

    void hienThiBan(){
        line(73);
        cout << "| "
             << left << setw(5)  << "Ma"
             << "| "
             << setw(15) << "Loai Ban"
             << "| "
             << setw(10) << "Gia"
             << "| "
             << setw(18) << "Khach"
             << "| "
             << setw(12) << "Trang Thai"
             << " |\n";
        line(73);
        for(auto b : dsBan.get())
            b.xuat();
        line(73);
    }
    
 void congDiemKhach(const string& ten, double tongTien){
    for(auto p : dsNguoi){
        KhachHang* kh = dynamic_cast<KhachHang*>(p);
        if(kh && kh->getTen() == ten){
            kh->congDiem(tongTien);
            return;
        }
    }

    cout << "\n[WARN] Khong tim thay khach hang de cong diem!\n";
}
void xemLichSuKhach(){
    string ten =
        nhapString("Nhap ten khach hang: ");
    KhachHang* kh =
        timKhachHang(ten);
    if(kh){
        kh->hienThiLichSu();
    }
    else{
        cout << "Khong tim thay khach hang!\n";
    }
}
	KhachHang* timKhachHang(const string& ten){
	    for(auto p : dsNguoi){
	        KhachHang* kh =
	            dynamic_cast<KhachHang*>(p);
	        if(kh && kh->getTen() == ten)
	            return kh;
	    }
	    return nullptr;
	}
    void hienThiDU(){
        line(43);
        cout << "| "
             << left << setw(5)  << "Ma"
             << "| "
             << setw(20) << "Ten"
             << "| "
             << setw(10) << "Gia"
             << " |\n";
        line(43);
        for(auto d : dsDU.get())
            d.xuat();
        line(43);
    }
    void thongKeDoanhThu(){
    double tong = 0;
    for(auto h : dsHD.get()){
        tong += h.getTongTien();
    } 
    line(40);
    cout << "TONG DOANH THU: "
         << tong
         << " VND\n";
    line(40);
    }
    void themNuocChoBan(BanBida &b){
        hienThiDU();
        int sl =
            nhapInt("Nhap so loai nuoc: ");
        for(int i=0;i<sl;i++){
            int maDU =
                nhapInt("Nhap ma do uong: ");
            int soLuong =
                nhapInt("Nhap so luong: ");
            bool check = false;
            for(auto d : dsDU.get()){
                if(d.getMa() == maDU){
                    double tien =
                        d.getGia() * soLuong;
                    b.themDoUong(
                        d.getTen(),
                        tien
                    );
                    check = true;
                }
            }
            if(!check)
                cout << "Khong tim thay do uong!\n";
        }
    }

    void themNguoi(){
        int chon = nhapInt("1.Hoi Vien 2.NV: \n");
        ConNguoi* p;
        if(chon == 1) p = new KhachHang();
        else p = new NhanVien();
        p->nhap();
        dsNguoi.push_back(p);
    }
    void hienThiNguoi(){
        // ===== KHACH HANG =====
        line(55);
        cout << "| "
             << left << setw(15) << "Ten"
             << "| "
             << setw(12) << "SDT"
             << "| "
             << setw(8)  << "Diem"
             << "| "
             << setw(10) << "Hang"
             << " |\n";
        line(55);
        for(auto p : dsNguoi){
            KhachHang* kh =
                dynamic_cast<KhachHang*>(p);
            if(kh)
                kh->xuat();
        }
        line(55);
        cout << endl;
        // ===== NHAN VIEN =====
        line(60);
        cout << "| "
             << left << setw(15) << "Ten"
             << "| "
             << setw(12) << "SDT"
             << "| "
             << setw(12) << "Chuc Vu"
             << "| "
             << setw(10) << "Luong"
             << " |\n";
        line(60);
        for(auto p : dsNguoi){
            NhanVien* nv =
                dynamic_cast<NhanVien*>(p);
            if(nv)
                nv->xuat();
        }
        line(60);
    }
    void moBan(){
        int ma =
            nhapInt("Nhap ma ban: ");
        string ten =
            nhapString("Nhap ten khach: ");
        int h =
            nhapInt("Nhap gio mo: ");
        while(h < 0 || h > 23)
            h = nhapInt("Nhap lai gio (0-23): ");
        int p =
            nhapInt("Nhap phut mo: ");
        while(p < 0 || p > 59)
            p = nhapInt("Nhap lai phut (0-59): ");
        for(auto &b : dsBan.get()){
            if(b.getMa() == ma){
                try{
                    b.moBan(ten,h,p);
                    char c;
                    cout << "Khach co goi nuoc khong? (y/n): ";
                    cin >> c;
                    if(c == 'y' || c == 'Y')
                        themNuocChoBan(b);
                    cout << "\nMo ban thanh cong!\n";
                }catch(...){
                    cout << "\nBan dang su dung!\n";
                }
                return;
            }
        }
        cout << "\nKhong tim thay ban!\n";
    }

    void themNuoc(){
        while(true){
            int maBan =
                nhapInt("Nhap ma ban: ");
            bool timThay = false;
            for(auto &b : dsBan.get()){
                if(b.getMa() == maBan){
                    timThay = true;
                // KIEM TRA BAN DA MO CHUA
                    if(!b.getTrangThai()){
                        cout << "\nBan chua mo!\n";
                        cout << "Vui long nhap lai.\n\n";
                        break;
                    }
                    themNuocChoBan(b);
                    cout << "\nThem nuoc thanh cong!\n";
                    return;
                }
            }
        if(!timThay){
            cout << "\nKhong tim thay ban!\n";
        }
        }
    }

    void dongBan(){
        int ma =
            nhapInt("Nhap ma ban: ");
        int h =
            nhapInt("Nhap gio dong: ");
        while(h < 0 || h > 23)
            h = nhapInt("Nhap lai gio (0-23): ");
        int p =
            nhapInt("Nhap phut dong: ");
            
        while(p < 0 || p > 59)
            p = nhapInt("Nhap lai phut (0-59): ");
        for(auto &b : dsBan.get()){
            if(b.getMa() == ma){
                try{
                    if(!b.getTrangThai())
                        throw runtime_error("Ban chua mo!");

                    string tenKhach =
                    b.getTenKhach();
                    string loai =
                        b.getLoai();
                    double giaBan =
                        b.getGia();
                    vector<pair<string,double>> nuoc =
                        b.getDSNuoc();
                    int hMo =
                        b.getHMo();
                    int pMo =
                        b.getPMo();
                    int mo =
                        hMo * 60 + pMo;
                    int dong =
                        h * 60 + p;
                    int tongPhut = dong - mo;
                    if(tongPhut <= 0){
                        cout << "\nGio dong phai lon hon gio mo!\n";
                        return;
                    }
                    double soGio =
                        tongPhut / 60.0;

                    double tienBan =
                        soGio * giaBan;
                    double tienNuoc = 0;
                    for(auto x : nuoc)
                        tienNuoc += x.second;
                    double tong =
						tienBan + tienNuoc;
						
					// ===== GIAM GIA KHACH HANG =====
						double giamGia = 0;
						
						KhachHang* kh =
						    timKhachHang(tenKhach);
						
						if(kh){
						    giamGia =
						        tong * kh->layGiamGia();
						    tong -= giamGia;
						}
						b.dongBan(h,p);
					HoaDon hd(
                        dsHD.get().size() + 1,
                        ma,
                        hMo,
                        pMo,
                        h,
                        p,
                        tong
                    );
                    dsHD.them(hd);
                    congDiemKhach(tenKhach, tong);
                    if(kh){
                        string lichSu =
                            "Ban " + to_string(ma) +
                            " | Tong tien: " + to_string((int)tong) +
                            " VND";
                        kh->themLichSu(lichSu);
                    }
                    cout << "\n";
                    line(60);
                    cout << setw(38)
                         << "HOA DON THANH TOAN\n";
                    line(60);
                    cout << left
                         << setw(25)
                         << "Ma ban:"
                         << ma << endl;
                    cout << left
                         << setw(25)
                         << "Loai ban:"
                         << loai << endl;
                    cout << left
                         << setw(25)
                         << "Gia ban / gio:"
                         << giaBan
                         << " VND\n";
                    cout << left
                         << setw(25)
                         << "Khach hang:"
                         << tenKhach << endl;
                    cout << left
                         << setw(25)
                         << "Gio mo:";
                    if(hMo < 10) cout << "0";
                    cout << hMo << ":";
                    if(pMo < 10) cout << "0";
                    cout << pMo << endl;
                    cout << left
                         << setw(25)
                         << "Gio dong:";
                    if(h < 10) cout << "0";
                    cout << h << ":";
                    if(p < 10) cout << "0";
                    cout << p << endl;

                    int gio = tongPhut / 60;
                    int phut = tongPhut % 60;
                    cout << left
                         << setw(25)
                         << "Tong thoi gian:"
                         << gio << " gio ";
                    if(phut > 0)
                        cout << phut << " phut";
                    cout << endl;
                    line(60);
                    cout << "DO UONG DA GOI\n";
                    line(60);
                    if(nuoc.empty()){
                        cout << "Khong co\n";
                    }else{
                        for(auto x : nuoc){
                            cout << left
                                 << setw(35)
                                 << x.first;
                            cout << x.second
                                 << " VND\n";
                        }
                    }
                    line(60);
                    cout << left
                         << setw(25)
                         << "Tien ban:"
                         << tienBan
                         << " VND\n";
                    cout << left
                         << setw(25)
                         << "Tien nuoc:"
                         << tienNuoc
                         << " VND\n";
                    cout << left
					     << setw(25)
					     << "Giam gia:"
					     << giamGia
					     << " VND\n";
                    cout << left
                         << setw(25)
                         << "Tong thanh toan:"
                         << tong
                         << " VND\n";
                        int diemCong = tong / 10000;
                    cout << left
                         << setw(25)
                         << "Diem duoc cong:"
                         << diemCong << " diem\n";
                        line(60);
                }catch(...){
                    cout << "\nBan chua mo!\n";
                    }
                    return;
            }
        }
            cout << "\nKhong tim thay ban!\n";
    }
    void hienThiHD(){
        line(45);
        cout << "| "
             << left << setw(5)  << "HD"
             << "| "
             << setw(5)  << "Ban"
             << "| "
             << setw(13) << "Thoi Gian"
             << "| "
             << setw(12) << "Tong Tien"
             << " |\n";
        line(45);
        for(auto h : dsHD.get())
            h.xuat();
        line(45);
    }
    void thongTinBan(){
        int ma =
            nhapInt("Nhap ma ban: ");
        for(auto &b : dsBan.get()){
            if(b.getMa() == ma){
                b.xuatChiTiet();
                return;
            }
        }
        cout << "Khong tim thay ban!\n";
    }
};

int main(){
    QuanLy ql;
    int chon;
    do{
        line(50);
        cout << "           QUAN LY QUAN BIDA\n";
        line(50);
        cout << "| 1. Danh sach ban                  |\n";
        cout << "| 2. Menu do uong                   |\n";
        cout << "| 3. Mo ban                         |\n";
        cout << "| 4. Them do uong                   |\n";
        cout << "| 5. Dong ban                       |\n";
        cout << "| 6. Danh sach hoa don              |\n";
        cout << "| 7. Thong tin 1 ban                |\n";
        cout << "| 8. Them nguoi                     |\n";
        cout << "| 9. Danh sach nguoi                |\n";
        cout << "| 10. Lich su khach hang            |\n";
        cout << "| 11. Thong ke doanh thu            |\n";
        cout << "| 0. Thoat                          |\n";
        line(50);
        chon =
            nhapInt("Nhap lua chon: ");
        switch(chon){
        case 1:
            ql.hienThiBan();
            break;
        case 2:
            ql.hienThiDU();
            break;
        case 3:
            ql.moBan();
            break;
        case 4:
            ql.themNuoc();
            break;
        case 5:
            ql.dongBan();
            break;
        case 6:
            ql.hienThiHD();
            break;
        case 7:
            ql.thongTinBan();
            break;
        case 8:
            ql.themNguoi();
            break;
        case 9:
            ql.hienThiNguoi();
            break;
        case 10:
            ql.xemLichSuKhach();
            break;
        case 11:
            ql.thongKeDoanhThu();
            break;
        case 0:
            cout << "\nThoat chuong trinh!\n";
            break;
        default:
            cout << "\nLua chon khong hop le!\n";
        }
    }while(chon != 0);
    return 0;
}
