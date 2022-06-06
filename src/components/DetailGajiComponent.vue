<template>
    <div id="detailGaji">
        <div class="card">
            <div class="card-body border-bottom-dashed">
                <h6>{{ datas.nama_karyawan }}</h6>
                <p class="mb-0 text-muted">
                    {{ printDate(datas.tanggal_awal) }} - {{ printDate(datas.tanggal_akhir) }}
                </p>
            </div>
        </div>

        <!-- Gaji -->
        <div class="card">
            <div class="card-body">
                <h6 class="mb-0">Gaji</h6>
            </div>
            <div class="card-body pt-0">
                <div v-for="gaji, indexGaji in datas.pengaturan_gaji" class="checkpoint-item" :key="'gaji-'+indexGaji">
                    <div>
                        <p class="mb-0 font-weight-500 mb-1">{{ gaji.nama }}</p>
                        <p class="small text-muted mb-0">
                            {{ printNominal(gaji.nominal) }} x
                            <span v-if="gaji.jenis == 'periode'">
                                {{ datas.total_periode }} periode
                            </span>
                            <span v-else>
                                {{ datas.total_kehadiran }} kehadiran
                            </span>
                        </p>
                    </div>
                    <p class="mb-0 font-500">{{ countTotalGaji(gaji.jenis, gaji.nominal) }}</p>
                </div>

                <div class="border-bottom-dashed"></div>
            </div>
            <div class="card-body d-flex align-items-center justify-content-between pt-0">
                <h6 class="mb-0">Subtotal Gaji</h6>
                <h6 class="mb-0">Rp. {{ subtotalGaji.toLocaleString('id') }}</h6>
            </div>
        </div>

        <!-- Upah Borongan -->
        <div class="card">
            <div class="card-body border-bottom">
                <h6 class="mb-0">Upah Borongan</h6>
            </div>
            <div class="card-body">
                <div v-for="upah in datas.upah_borongan" class="checkpoint-item" :key="'upah-'+upah.id">
                    <div>
                        <p class="mb-0 font-weight-500">{{ capitalize(upah.nama) }}</p>
                        <p class="small text-muted mb-0">
                            {{ upah.jumlah_pengerjaan }}{{ upah.satuan }}
                        </p>
                    </div>
                    <p class="mb-0 font-500">{{ countTotalUpah(upah.jumlah_pengerjaan, upah.nominal) }}</p>
                </div>

                <div class="border-bottom-dashed"></div>
            </div>
            <div class="card-body d-flex align-items-center justify-content-between pt-0">
                <h6 class="mb-0">Subtotal Upah</h6>
                <h6 class="mb-0">Rp. {{ subtotalUpah.toLocaleString('id') }}</h6>
            </div>
        </div>

        <!-- Komisi -->
        <div class="card">
            <div class="card-body border-bottom">
                <h6 class="mb-0">Komisi</h6>
            </div>
            <div class="card-body">
                <div v-if="datas.komisi">
                    <div v-for="k, indexKomisi in datas.komisi" class="checkpoint-item mb-3" :key="'komisi-' + indexKomisi">
                        <p class="mb-0 font-weight-500">{{ k.nama }}</p>
                        <p class="mb-0 font-500">{{ printNominal(k.nominal) }}</p>
                    </div>
                </div>

                <div v-if="datas.komisi" class="border-bottom-dashed"></div>
            </div>
            <div class="card-body d-flex align-items-center justify-content-between pt-0">
                <h6 class="mb-0">Subtotal Komisi</h6>
                <h6 class="mb-0">
                    Rp. {{ subtotalKomisi.toLocaleString('id') }}
                </h6>
            </div>
        </div>

        <!-- Total Gaji Kotor -->
        <div class="card">
            <div class="card-body d-flex align-items-center justify-content-between text-green">
                <h6 class="mb-0">Total Gaji Kotor</h6>
                <h6 class="mb-0">Rp. {{ totalGajiKotor.toLocaleString('id') }}</h6>
            </div>
        </div>

        <!-- Tanggungan -->
        <div class="card">
            <div class="card-body border-bottom">
                <h6 class="mb-2">Tanggungan</h6>
                <p class="small text-muted mb-0">
                    Karyawan ini memiliki tanggungan Rp. 570.000
                </p>
            </div>
            <div class="card-body">
                <div v-if="datas.tanggungan">
                    <div v-for="t, indexTanggungan in datas.tanggungan" class="checkpoint-item mb-3" :key="'tanggungan-' + indexTanggungan">
                        <div>
                            <p class="mb-0 font-weight-500">{{ t.nama }}</p>
                            <p class="small text-muted mb-0">{{ t.keterangan }}</p>
                        </div>
                        <p class="mb-0 font-500">{{ printNominal(parseInt(t.nominal)) }}</p>
                    </div>
                </div>

                <div v-if="datas.tanggungan" class="border-bottom-dashed"></div>
            </div>
            <div class="card-body d-flex align-items-center justify-content-between pt-0">
                <h6 class="mb-0">Tanggungan Dibayar</h6>
                <h6 class="mb-0 text-red">(-) Rp. {{ subtotalTanggungan.toLocaleString('id') }}</h6>
            </div>
        </div>

        <!-- Total Gaji Bersih -->
        <div class="card">
            <div class="card-body text-blue">
                <div class="d-flex align-items-center justify-content-between p-0 mb-2">
                    <h6 class="mb-0">
                        Total Gaji Bersih
                        <i class="fa-solid fa-circle-check fa-xs text-green"></i>
                    </h6>
                    <h6 class="mb-0">Rp. {{ totalGajiBersih.toLocaleString('id') }}</h6>
                </div>
                <p class="text-muted small">
                    Nominal akhir yang diterima karyawan setelah ditambah komisi
                    dikurangi pembayaran tanggungan (jika ada)
                </p>
            </div>
        </div>

        <!-- Keterangan -->
        <div class="card">
            <div class="card-body">
                <p class="mb-1 small text-muted">Keterangan</p>
                <p class="font-weight-500 bigger pb-3">Lebih Semangat Lagi Ya!</p>

                <button type="button" class="btn button-cetak mt-3">
                    Cetak
                </button>
            </div>
        </div>
    </div>
</template>

<style scoped>

#detailGaji {
    background-color: #f2f5f7;
}

    .card {
        border: 0;
        border-radius: 0;
        background-color: #fff;
        margin-bottom: 1rem;
    }

        .card-body {
            padding: 20px;
            text-align: left;
        }

        .border-bottom-dashed {
            border-bottom: 2px dashed #ddd;
        }

        .border-bottom {
            border-bottom: 1px solid #ddd;
        }

        .checkpoint-item {
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        .checkpoint-item {
            width: 100%;
            margin-bottom: 1rem;
        }

        .checkpoint-item:last-child {
            margin-bottom: 0;
        }

        .button-cetak {
            background-color: #00a88a;
            border-radius: 3px;
            color: #fff;

            display: block;
            width: 100%;
        }

input, select {
    border-radius: 2px;
}

</style>

<script>
import jQuery from "jquery";

const $ = jQuery;
window.$ = $;

export default {
    name: 'DetailGajiComponent',
    data() {
        return {
            datas: [],
        }
    },
    computed: {
        durasiKeterlambatan() {
            return `${this.datas.total_kehadiran} Hari`;
        },
        subtotalGaji() {
            let total = 0;
            let subtotal = 0;
            let getGaji = this.datas.pengaturan_gaji;
            let self = this;

            if(getGaji) {
                getGaji.map(g => {
                    if(g.jenis == 'periode') {
                        subtotal = g.nominal * self.datas.total_periode;
                    } else {
                        subtotal = g.nominal * self.datas.total_kehadiran;
                    }

                    total += subtotal
                });

                return total;
            }

            return '';
        },
        subtotalUpah() {
            let total = 0;
            let subtotal = 0;
            let getUpah = this.datas.upah_borongan;

            if(getUpah) {
                getUpah.map(g => {
                    subtotal = g.nominal * g.jumlah_pengerjaan;

                    total += subtotal
                });

                return total;
            }

            return '';
        },
        subtotalKomisi() {
            let getKomisi = this.datas.komisi;

            if(getKomisi) {
                let total = 0;
                getKomisi.map(g => {
                    total += parseInt(g.nominal);
                });

                return total;
            } else {
                return 0;
            }
        },
        subtotalTanggungan() {
            let getTanggungan = this.datas.tanggungan;
            let tanggunganAkhir = 0;

            if(getTanggungan && getTanggungan.length > 0) {
                getTanggungan.map(g => {
                    tanggunganAkhir += parseInt(g.nominal);
                });
            }

            return tanggunganAkhir;
        },
        totalGajiKotor() {
            let total = this.subtotalGaji + this.subtotalUpah + this.subtotalKomisi;
            return total;
        },
        totalGajiBersih() {
            let total = this.totalGajiKotor - (570000 - parseInt(this.subtotalTanggungan));
            if(this.subtotalTanggungan == 0) {
                total = this.totalGajiKotor - 570000;
            }

            return total;
        },
        durasiText() {
            let total_kehadiran = this.datas.total_kehadiran;
            return `${total_kehadiran} Hari`;
        }
    },
    methods: {
        capitalize(str) {
            return str.charAt(0).toUpperCase()+str.slice(1);
        },
        fetchData() {
            let self = this;
            $.ajax({
                url: 'https://boss.smartlink.id/salary/inquiry',
                type: 'GET',
                dataType: 'JSON',
                success: function(res) {
                    let data = res.data;

                    self.datas = data;
                    
                    let getPengaturanUpah = self.datas.pengaturan_upah;
                    let getPengerjaanUpah = self.datas.pengerjaan_upah;
                    self.datas.upah_borongan = [];

                    // mengelompokkan upah mjd 1
                    // supaya gampang ketika dilooping
                    getPengaturanUpah.map((upah, index) => {    
                        let findUpah = getPengerjaanUpah.find(val => {
                            return val.pengaturan_upah_id == upah.id;
                        });

                        // change object key name
                        findUpah['jumlah_pengerjaan'] = findUpah['nominal'];
                        delete findUpah['nominal'];

                        // convert javascript array to object
                        // https://attacomsian.com/blog/javascript-convert-array-to-object
                        let objPengaturan = Object.assign({}, upah);
                        let objPengerjaan = Object.assign({}, findUpah);

                        let objUpah = {...objPengaturan, ...objPengerjaan}
                        self.datas.upah_borongan[index] = objUpah;
                    });

                    // set total kehadiran default (untuk data di modal)
                    self.total_kehadiran_default = data.total_kehadiran;
                    self.periode_default = data.total_periode;

                    $('#inputGapok').val(self.datas.pengaturan_gaji[0].nominal.toLocaleString('id'));

                    console.log(self.datas);
                },
                fail: function(err) {
                    console.error('Error fetching data : ', err);
                }
            });
        },
        printDate(date) {
            let dt = new Date(date);
            let listBulan = [
                'Januari', 'Februari', 'Maret', 'April',
                'Mei', 'Juni', 'Juli', 'Agustus',
                'September', 'Oktober', 'November', 'Desember'
            ];

            let tgl = dt.getDate();
            if(tgl < 10) {
                tgl = '0' + tgl;
            }

            let bulan = listBulan[dt.getMonth()];
            let tahun = dt.getFullYear();

            let dateStr = `${tgl} ${bulan} ${tahun}`
            return dateStr;
        },
        printNominal(nominal) {
            return parseInt(nominal).toLocaleString('id');
        },
        countTotalUpah(jml, nominal) {
            let total = jml * nominal;
            return total.toLocaleString('id');
        },
        countTotalGaji(jenis, nominal) {
            let total = 0;
            if(jenis == 'periode') {
                total = nominal * this.datas.total_periode
                return total.toLocaleString('id');
            } else {
                total = nominal * this.datas.total_kehadiran;
                return total.toLocaleString('id');
            }
        },
    },
    mounted() {
        this.fetchData();
    }
}
</script>