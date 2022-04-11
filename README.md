# Tugas-CRUD
#ramdhan M Fajarudin
import psycopg2 as db
import os

con = None
connected = None
cursor = None

def connect():
    global connected
    global con
    global cursor
    try:
        con = db.connect(
        host = 'localhost', 
        database ="univ",
        port = 5432,
        user = "mhmd",
        password = "123"
        )
        cursor = con.cursor()
        connected = True
    except:
        connected = False
    return cursor

def disconnect():
    global connected
    global con
    global cursor
    if (connected == True):
        cursor.close()
        con.close()
    else:
        con = None
        connected = False
        
def Entry():
    global connected
    global con
    global cursor
    xnim = input("Masukkan NIM : ")
    xnama = input("Masukkan Nama Lengkap : ")
    xidfk = input("Masukkan ID Fakultas (1 .. 5) : ")
    xidpr = input("Masukkan ID Prodi (1 .. 10) : ")
    a = connect()
    sql = "insert into mahasiswa (nim, nama, idfakultas, idprodi) values ('"+xnim+"','"+xnama+"','"+xidfk+"','"+xidpr+"')"
    a.execute(sql)
    con.commit()
    print("Entry is done.")

def Cari():
    global connected
    global con
    global cursor
    xnim = input("Masukkan NIM yang dicari : ")
    a = connect()
    sql = "select * from mahasiswa where nim ='"+xnim+"'"
    a.execute(sql)
    record = a.fetchall()
    print(record)
    print("Search is done.")
    
def Tampilkan():
    global connected
    global con
    global cursor
    a = connect ()
    sql =  "select * from mahasiswa"
    a.execute(sql)
    record = a.fetchall()
    print ("Data yang Tersimpan: ")
    print(record)
  

def Ubah():
    global connected
    global con
    global cursor
    xnim = input("Masukkan NIM yang dicari : ")
    a = connect()
    sql = "select * from mahasiswa where nim ='"+xnim+"'"
    a.execute(sql)
    record = a.fetchall()
    print("Data saat ini : ")
    print(record)
    row = a.rowcount
    if(row==1):
        print("Silahkan untuk mengubah data..")
        xnama = input("Masukkan Nama Lengkap : ")
        xidfk = input("Masukkan ID Fakultas (1 .. 5) : ")
        xidpr = input("Masukkan ID Prodi (1 .. 10) : ")
        a = connect()
        sql = "Update mahasiswa set nama='"+xnama+"', idfakultas='"+xidfk+"', idprodi='"+xidpr+"' where nim='"+xnim+"'"
        a.execute(sql)
        con.commit()
        print("Update is done.")
        sql = "select * from mahasiswa where nim ='"+xnim+"'"
        a.execute(sql)
        record = a.fetchall()
        print("Data setelah diubah : ")
        print(record)

    else:
        print("Data tidak ditemukan")

def Hapus():
    global connected
    global con
    global cursor
    xnim = input("Masukkan NIM yang dicari : ")
    a = connect()
    sql = "select * from mahasiswa where nim ='"+xnim+"'"
    a.execute(sql)
    record = a.fetchall()
    print("Data saat ini : ")
    print(record)
    row = a.rowcount
    if(row==1):
        jwb=input("Apakah anda ingin menghapus data? (y/t)")
        if(jwb.upper()=="Y"):
            a = connect()
            sql = "delete from mahasiswa where nim ='"+xnim+"'"
            a.execute(sql)
            con.commit()
            print("Delete is done.")
        else:
            print("Data batal untuk dihapus.")
    else:
        print("Data tidak ditemukan")
        
def show_menu():
    print("\n+++++====++++ APLIKASI DATA MAHASISWA ++++====++++++")
    print("""Apa yang ingin anda Lakukan?
            1. Memasukan (Membuat) Data baru
            2. Mencari Data
            3. Mengubah Data
            4. Menghapus Data
            5. Menampilkan Data
            0. Keluar""")
      
    menu = input("Pilih menu> ")
    os.system("cls")

    if menu == "1":
        Entry()
    elif menu == "2":
        Cari()
    elif menu == "3":
        Ubah()
    elif menu == "4":
        Hapus()
    elif menu == "5":
        Tampilkan()
    elif menu == "0":
        exit()
    else:
        print("Periksa Kembali Pilihan Anda, dan Coba Lagi")
    if __name__ == "__main__":
        while(True):
            show_menu()
show_menu()
