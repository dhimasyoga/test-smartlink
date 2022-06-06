<template>
    <div id="faktur">
        <div class="modal-overlay"></div>

        <!-- Modal Kehadiran -->
        <div class="modal-custom" id="modalKehadiran">
            <div class="modal-custom-content">
                <div class="modal-custom-header border-bottom p-3 position-relative text-center">
                    <h5 class="mb-0">Ubah Kehadiran</h5>
                    <i  @click="hideModal('#modalKehadiran')"
                        class="fa-solid fa-xmark fa-lg modal-close-icon text-blue hover-pointer"></i>
                </div>
                <div class="modal-custom-body p-3">
                    <label for="durasi" class="small text-left text-muted mb-1">Durasi Keterlambatan</label>
                    <div class="input-group mb-0">
                        <button class="btn btn-outline-secondary text-blue" type="button" @click="datas.total_kehadiran--">
                            <i class="fa-solid fa-minus fa-xs"></i>
                        </button>
                        <input type="text" class="form-control text-center" v-model="durasiKeterlambatan" autofocus>
                        <button class="btn btn-outline-secondary text-blue" type="button" @click="datas.total_kehadiran++">
                            <i class="fa-solid fa-plus fa-xs"></i>
                        </button>
                    </div>
                </div>
                <div class="modal-custom-footer pb-3 px-3 d-flex align-items-center justify-content-between" style="gap: 10px">
                    <button type="button" @click="datas.total_kehadiran = 0" class="btn btn-outline-danger d-block w-100">
                        Hapus
                    </button>
                    <button type="button" @click="updateKehadiran()" class="btn btn-primary d-block w-100">
                        Simpan
                    </button>
                </div>
            </div>
        </div>

        <!-- Modal Gaji Pokok -->
        <div class="modal-custom" id="modalGajiPokok">
            <div class="modal-custom-content">
                <div class="modal-custom-header border-bottom p-3 position-relative text-center">
                    <h5 class="mb-0">Ubah Gaji Pokok</h5>
                    <i  @click="hideModal('#modalGajiPokok')"
                        class="fa-solid fa-xmark fa-lg modal-close-icon text-blue hover-pointer"></i>
                </div>
                <div class="modal-custom-body p-3">
                    <label for="nominal" class="small text-left text-muted mb-1">Nominal</label>
                    <div class="d-flex align-items-center" style="gap: 10px">
                        <div class="input-group input-group-sm mb-0">
                            <span class="input-group-text" id="inputGroup-sizing-sm">Rp</span>
                            <input type="text" class="form-control input-angka input-gaji" @input="countSubtotalGapok()" id="inputGapok" autocomplete="off" autofocus>
                        </div>

                        <div class="input-group input-group-sm mb-0">
                            <input type="number" class="form-control" v-model="periode_default" @input="countSubtotalGapok()" autocomplete="off" min="1">
                            <span class="input-group-text" id="inputGroup-sizing-sm">Periode</span>
                        </div>
                    </div>

                    <div class="border-bottom-dashed my-3"></div>

                    <div class="d-flex align-items-center justify-content-between">
                        <p class="mb-0">Jumlah</p>

                        <p class="mb-0">Rp. <span class="subtotal-gapok"></span></p>
                    </div>
                </div>
                <div class="modal-custom-footer pb-3 px-3">
                    <button type="button" @click="updateGajiPokok()" class="btn btn-primary btn-block w-100">
                        Simpan
                    </button>
                </div>
            </div>
        </div>

        <!-- Modal Gaji Tunjangan -->
        <div class="modal-custom" id="modalGajiTunjangan">
            <div class="modal-custom-content">
                <div class="modal-custom-header border-bottom p-3 position-relative text-center">
                    <h5 class="mb-0">
                        Ubah
                        <span class="tunjangan-title">Uang Makan</span>
                    </h5>
                    <i  @click="hideModal('#modalGajiTunjangan')"
                        class="fa-solid fa-xmark fa-lg modal-close-icon text-blue hover-pointer"></i>
                </div>
                <div class="modal-custom-body p-3">
                    <label for="nominal" class="small text-left text-muted mb-1">Nominal</label>
                    <div class="d-flex align-items-center" style="gap: 10px">
                        <div class="input-group input-group-sm mb-0 w-50">
                            <span class="input-group-text" id="inputGroup-sizing-sm">Rp</span>
                            <input type="text" class="form-control input-gaji" @input="countSubtotalTunjangan()" id="inputTunjangan" autocomplete="off" autofocus>
                        </div>

                        <div class="small" style="flex-shrink: 0">
                            x {{ datas.total_kehadiran }} Hari
                        </div>
                    </div>

                    <div class="border-bottom-dashed my-3"></div>

                    <div class="d-flex align-items-center justify-content-between">
                        <p class="mb-0">Jumlah</p>

                        <p class="mb-0">Rp. <span class="subtotal-tunjangan"></span></p>
                    </div>
                </div>
                <div class="modal-custom-footer pb-3 px-3">
                    <button type="button" @click="updateGajiTunjangan()" class="btn btn-primary btn-block w-100">
                        Simpan
                    </button>
                </div>
            </div>
        </div>

        <!-- Modal Komisi -->
        <div class="modal-custom" id="modalKomisi">
            <div class="modal-custom-content">
                <div class="modal-custom-header border-bottom p-3 position-relative text-center">
                    <h5 class="mb-0 komisi-title"></h5>
                    <i  @click="hideModal('#modalKomisi')"
                        class="fa-solid fa-xmark fa-lg modal-close-icon text-blue hover-pointer"></i>
                </div>
                <form action="" id="formKomisi" @submit.prevent="updateKomisi()">
                    <div class="modal-custom-body p-3">
                        <input type="hidden" v-model="indexKomisi">

                        <label for="nominal" class="small text-left text-muted mb-1">Nama Komisi</label>
                        <input type="text" placeholder="Nama Komisi" class="form-control input-nama-komisi mb-3" autocomplete="off" required autofocus>

                        <label for="nominal" class="small text-left text-muted mb-1">Nominal</label>
                        <div class="input-group input-group-sm mb-0">
                            <span class="input-group-text" id="inputGroup-sizing-sm">Rp</span>
                            <input type="text" class="form-control input-angka input-nominal-komisi" autocomplete="off" required>
                        </div>
                    </div>
                    <div class="modal-custom-footer pb-3 px-3 d-flex align-items-center justify-content-between" style="gap: 10px">
                        <button v-if="indexKomisi !== ''" type="button" @click="deleteKomisi()" class="btn btn-outline-danger btn-delete-komisi d-block w-100">
                            Hapus
                        </button>

                        <button class="btn btn-primary d-block w-100">
                            Simpan
                        </button>
                    </div>
                </form>
            </div>
        </div>

        <!-- Modal Tanggungan -->
        <div class="modal-custom" id="modalTanggungan">
            <div class="modal-custom-content">
                <div class="modal-custom-header border-bottom p-3 position-relative text-center">
                    <h5 class="mb-0 tanggungan-title"></h5>
                    <i  @click="hideModal('#modalTanggungan')"
                        class="fa-solid fa-xmark fa-lg modal-close-icon text-blue hover-pointer"></i>
                </div>
                <form action="" id="formTanggungan" @submit.prevent="updateTanggungan()">
                    <div class="modal-custom-body p-3">
                        <input type="hidden" v-model="indexTanggungan">

                        <label for="namaTanggungan" class="small text-left text-muted mb-1">Nama Tanggungan</label>
                        <input type="text" id="namaTanggungan" placeholder="Contoh : Ganti Barang Hilang" class="form-control input-nama-tanggungan mb-3" autocomplete="off" required autofocus>

                        <label for="nominalTanggungan" class="small text-left text-muted mb-1">Nominal Pembayaran</label>
                        <div class="input-group input-group-sm mb-3">
                            <span class="input-group-text" id="inputGroup-sizing-sm">Rp</span>
                            <input type="text" id="nominalTanggungan" class="form-control input-angka input-nominal-tanggungan" autocomplete="off" required>
                        </div>

                        <label for="keteranganTanggungan" class="small text-left text-muted mb-1">Keterangan</label>
                        <input type="text" id="keteranganTanggungan" placeholder="Contoh : Baju yang hilang warna merah" class="form-control input-keterangan-tanggungan mb-3" autocomplete="off" required>
                    </div>
                    <div class="modal-custom-footer pb-3 px-3 d-flex align-items-center justify-content-between" style="gap: 10px">
                        <button v-if="indexTanggungan !== ''" type="button" @click="deleteTanggungan()" class="btn btn-outline-danger btn-delete-tanggungan d-block w-100">
                            Hapus
                        </button>

                        <button class="btn btn-primary d-block w-100">
                            Simpan
                        </button>
                    </div>
                </form>
            </div>
        </div>


        <Transition name="slide" mode="out-in">
            <div v-if="stepPenggajian == 1" :key="1">
                <div class="card">
                    <div class="card-body border-bottom-dashed">
                        <h6>{{ datas.nama_karyawan }}</h6>
                        <p class="mb-0 text-muted">
                            {{ printDate(datas.tanggal_awal) }} - {{ printDate(datas.tanggal_akhir) }}
                        </p>
                    </div>
                    <div class="card-body d-flex align-items-center justify-content-between">
                        <p class="mb-0">
                            Masuk {{ datas.total_kehadiran }} Hari
                        </p>
                        <p class="change-presence mb-0" @click="showModal('#modalKehadiran')">Ubah Kehadiran</p>
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
                            <div class="d-flex align-items-center mb-0">
                                <p class="mb-0 font-500">{{ countTotalGaji(gaji.jenis, gaji.nominal) }}</p>

                                <i v-if="indexGaji == 0" @click="showModal('#modalGajiPokok', '', gaji.nominal)" class="fa-solid fa-pen-to-square fa-xs text-blue ms-2 hover-pointer"></i>
                                <i v-else @click="showModal('#modalGajiTunjangan', gaji.nama, gaji.nominal)" class="fa-solid fa-pen-to-square fa-xs text-blue ms-2 hover-pointer"></i>
                            </div>
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
                            <div class="d-flex align-items-center mb-0">
                                <p class="mb-0 font-500">{{ countTotalUpah(upah.jumlah_pengerjaan, upah.nominal) }}</p>
                                <i class="fa-solid fa-ban fa-xs text-muted ms-2 hover-pointer"></i>
                            </div>
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
                        <div    @click="showModal('#modalKomisi', 'Tambah Komisi', '', '')"
                                class="d-flex align-items-center text-blue hover-pointer mb-3">
                            <i class="fa-solid fa-circle-plus fa-xs"></i>
                            <p class="mb-0 ms-1">Tambah Komisi Lain...</p>
                        </div>

                        <div v-if="datas.komisi">
                            <div v-for="k, indexKomisi in datas.komisi" class="checkpoint-item mb-3" :key="'komisi-' + indexKomisi">
                                <p class="mb-0 font-weight-500">{{ k.nama }}</p>
                                <div class="d-flex align-items-center mb-0">
                                    <p class="mb-0 font-500">{{ printNominal(k.nominal) }}</p>
                                    <i  @click="showModal('#modalKomisi', 'Edit Komisi', '', indexKomisi)"
                                        class="fa-solid fa-pen-to-square fa-xs text-blue ms-2 hover-pointer"></i>
                                </div>
                            </div>
                        </div>

                        <div class="border-bottom-dashed"></div>
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
                        <div    @click="showModal('#modalTanggungan', 'Tambah Tanggungan', '', '')"
                                class="d-flex align-items-center text-blue hover-pointer mb-3">
                            <i class="fa-solid fa-circle-plus fa-xs"></i>
                            <p class="mb-0 ms-1">Tambah pembayaran tanggungan...</p>
                        </div>

                        <div v-if="datas.tanggungan">
                            <div v-for="t, indexTanggungan in datas.tanggungan" class="checkpoint-item mb-3" :key="'tanggungan-' + indexTanggungan">
                                <div>
                                    <p class="mb-0 font-weight-500">{{ t.nama }}</p>
                                    <p class="small text-muted mb-0">{{ t.keterangan }}</p>
                                </div>
                                <div class="d-flex align-items-center mb-0 text-red">
                                    <p class="mb-0 font-500">{{ printNominal(parseInt(t.nominal)) }}</p>
                                    <i  @click="showModal('#modalTanggungan', 'Ubah Tanggungan', '', indexTanggungan)"
                                        class="fa-solid fa-pen-to-square fa-xs ms-2 hover-pointer"></i>
                                </div>
                            </div>
                        </div>

                        <div class="border-bottom-dashed"></div>
                    </div>
                    <div class="card-body d-flex align-items-center justify-content-between pt-0">
                        <h6 class="mb-0">Tanggungan Dibayar</h6>
                        <h6 class="mb-0 text-red">(-) Rp. {{ subtotalTanggungan.toLocaleString('id') }}</h6>
                    </div>
                </div>

                <!-- Total Gaji Bersih -->
                <div class="card mb-0">
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
                        <button class="btn-berikutnya" @click="stepPenggajian++">
                            Berikutnya
                        </button>
                    </div>
                </div>
            </div>

            <div v-else :key="2">
                <div class="card">
                    <div class="card-body pb-0">
                        <div class="d-flex align-items-center justify-content-between mb-2">
                            <p class="font-weight-500 mb-0">Subtotal Gaji</p>
                            <p class="font-weight-600 mb-0">Rp. {{ printNominal(parseInt(subtotalGaji)) }}</p>
                        </div>
                        <div class="d-flex align-items-center justify-content-between mb-2">
                            <p class="font-weight-500 mb-0">Subtotal Upah</p>
                            <p class="font-weight-600 mb-0">Rp. {{ printNominal(parseInt(subtotalUpah)) }}</p>
                        </div>
                        <div class="d-flex align-items-center justify-content-between">
                            <p class="font-weight-500 mb-0">Subtotal Komisi</p>
                            <p class="font-weight-600 mb-0">Rp. {{ printNominal(parseInt(subtotalKomisi)) }}</p>
                        </div>

                        <div class="d-flex align-items-center justify-content-between my-3">
                            <p class="font-weight-500 mb-0">Gaji Kotor</p>
                            <p class="font-weight-600 mb-0 text-green">Rp. {{ printNominal(parseInt(totalGajiKotor)) }}</p>
                        </div>

                        <div class="d-flex align-items-center justify-content-between my-3 text-red">
                            <p class="font-weight-500 mb-0">Tanggungan</p>
                            <p class="font-weight-600 mb-0">(-) Rp. {{ printNominal(parseInt(subtotalTanggungan)) }}</p>
                        </div>
                    </div>

                    <div class="border-bottom-dashed my-3"></div>

                    <div class="card-body pt-0">
                        <div class="d-flex align-items-center justify-content-between text-blue font-weight-600">
                            <p class="mb-0 bigger">Total Gaji Bersih</p>
                            <p class="mb-0 bigger">Rp. {{ printNominal(parseInt(totalGajiBersih)) }}</p>
                        </div>
                    </div>
                </div>

                <div class="card mb-0">
                    <div class="card-body pb-3">
                        <label for="namaBank" class="text-muted small">
                            Bayar dari Rekening
                            <span class="text-blue">*</span>
                        </label>
                        <select name="nama_bank" class="form-control mb-3 small" id="namaBank" v-model="selectedRekening" placeholder="Pilih Rekening Bank">
                            <option value="" disabled selected>Pilih Rekening Bank</option>
                            <option v-for="rek, indexRek in listRekening" :key="'rek-' + indexRek" :value="indexRek">
                                {{ rek.bank }} - {{ rek.nomor }}
                            </option>
                        </select>

                        <label for="tanggal" class="text-muted small">
                            Dicatat pada Tanggal
                            <span class="text-blue">*</span>
                        </label>
                        <input type="date" class="form-control mb-3 small" name="tanggal" id="tanggal" v-model="tanggalGaji" placeholder="Pilih Tanggal">

                        <label for="keterangan" class="text-muted small">Keterangan</label>
                        <input type="text" class="form-control mb-3 small" name="keterangan" id="keterangan" v-model="keteranganGaji" placeholder="Tambah Keterangan...">

                        <button type="button" @click="submitGaji()" class="btn btn-primary btn-block w-100 mt-3">
                            Submit Gaji
                        </button>
                    </div>
                </div>
            </div>
        </Transition>
    </div>
</template>

<style scoped>

.slide-enter-active {
  transition: all 0.5s ease-out;
}

.slide-leave-active {
  transition: all 0.5s ease-in;
}

.slide-enter-from,
.slide-leave-to {
  transform: translateX(20px);
  opacity: 0;
}

.change-presence {
    color: #0052f5;
    cursor: pointer;

    font-weight: 500;
}

#faktur {
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

        .btn-berikutnya {
            display: block;
            border: 0;
            color: #fff;
            background-color: #0052f5;

            padding: 15px;
            margin-top: 1rem;
            width: 100%;
        }

.modal-custom {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 3;

    width: 30%;
}

    .modal-close-icon {
        position: absolute;
        right: 20px;

        top: 50%;
        transform: translateY(-50%);
    }

    .modal-custom-content {
        background-color: #fff;
        border-radius: .5rem .5rem 0 0;
        z-index: 4;
    }

        .btn-outline-secondary {
            border: 1px solid #ced4da !important;
        }
        .btn-outline-secondary:hover {
            color: #fff !important;
        }

.modal-overlay {
    background-color: rgba(0,0,0,.8);
    position: fixed;
    top: 0;
    left: 0;

    width: 100%;
    height: 100vw;
    z-index: 2;
}

input, select {
    border-radius: 2px;
}

@media only screen and (max-width: 820px) {
   .modal-custom {
        width: 50%;
    }  

        .modal-overlay {
            height: 100%;
        }
}


@media only screen and (max-width: 480px) {
    .modal-custom {
        width: 95%;
    }
}

</style>

<script>
import jQuery from "jquery";

const $ = jQuery;
window.$ = $;

export default {
    name: 'FakturGajiComponent',
    data() {
        return {
            stepPenggajian: 0,
            total_kehadiran_default: '',
            periode_default: '',
            indexKomisi: '',
            indexTanggungan: '',

            listRekening: [],
            alreadyFetchBank: false,
            selectedRekening: '',
            tanggalGaji: '',
            keteranganGaji: '',

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
        hideModal(modalName) {
            typeof modalName !== 'undefined' ? modalName : '.modal-custom';

            $(modalName).fadeOut(200, function() {
                $('.modal-overlay').fadeOut(300);
                $('body').css('overflow-y', 'auto');
            });

            if(modalName == '#modalKehadiran') {
                this.datas.total_kehadiran = this.total_kehadiran_default;
            } else if(modalName == '#modalGajiPokok') {
                $('#inputGapok').val(parseInt(this.datas.pengaturan_gaji[0].nominal).toLocaleString('id'));
                this.periode_default = this.datas.total_periode;
            } else if(modalName == '#modalKomisi') {
                this.indexKomisi = '';
                $('#formKomisi')[0].reset();
            } else if(modalName == '#modalTanggungan') {
                this.indexTanggungan = '';
                $('#formTanggungan')[0].reset(); 
            }
        },
        showModal(modalName, modalTitle, nominal, index) {
            $('body').css('overflow-y', 'hidden');

            if(modalName == '#modalKehadiran') {
                this.total_kehadiran_default = this.datas.total_kehadiran;
            } else if(modalName == '#modalGajiPokok') {
                // $('#inputGapok').val(parseInt(nominal).toLocaleString('id'));
                this.countSubtotalGapok();
            } else if(modalName == '#modalGajiTunjangan') {
                $('.tunjangan-title').text(modalTitle);
                $('#inputTunjangan').val(parseInt(nominal).toLocaleString('id'));

                this.countSubtotalTunjangan();
            } else if(modalName == '#modalKomisi') {
                $('.komisi-title').text(modalTitle);
                this.indexKomisi = index;

                if(this.indexKomisi !== '') {
                    let dataKomisi = this.datas.komisi;
                    $('.input-nama-komisi').val(dataKomisi[this.indexKomisi].name);
                    $('.input-nominal-komisi').val(dataKomisi[this.indexKomisi].nominal);
                }
            } else if(modalName == '#modalTanggungan') {
                $('.tanggungan-title').text(modalTitle);
                this.indexTanggungan = index;

                if(this.indexTanggungan !== '') {
                    let dataTanggungan = this.datas.tanggungan;
                    $('.input-nama-tanggungan').val(dataTanggungan[this.indexTanggungan].name);
                    $('.input-nominal-tanggungan').val(dataTanggungan[this.indexTanggungan].nominal);
                    $('.input-keterangan-tanggungan').val(dataTanggungan[this.indexTanggungan].keterangan);
                }
            }

            $('.modal-overlay').fadeIn(200);
            $(modalName).fadeIn(300);

            $('[autofocus]').focus();
        },
        countSubtotalGapok() {
            let nominalTunjangan = parseInt($('#inputGapok').val().replace('.', ''));
            let subtotal = nominalTunjangan * this.periode_default;
            
            $('.subtotal-gapok').text(subtotal.toLocaleString('id'));
        },
        countSubtotalTunjangan() {
            let nominalTunjangan = parseInt($('#inputTunjangan').val().replace('.', ''));
            let subtotal = nominalTunjangan * this.datas.total_kehadiran;
            
            $('.subtotal-tunjangan').text(subtotal.toLocaleString('id'));
        },
        updateKehadiran() {
            this.total_kehadiran_default = this.datas.total_kehadiran;
            this.hideModal('#modalKehadiran');
        },
        updateGajiPokok() {
            this.datas.pengaturan_gaji[0].nominal = parseInt($('#inputGapok').val().replace('.', ''));
            this.datas.total_periode = this.periode_default;
            this.hideModal('#modalGajiPokok');
        },
        updateGajiTunjangan() {
            let jenis = $('.tunjangan-title').text();
            let newNominal = parseInt($('#inputTunjangan').val().replace('.', ''));

            let listGaji = this.datas.pengaturan_gaji;
            let findIndexGaji = listGaji.map(l => {
                return l.nama
            }).indexOf(jenis);

            this.datas.pengaturan_gaji[findIndexGaji].nominal = newNominal;
            this.hideModal('#modalGajiTunjangan');
        },
        updateKomisi() {
            let namaKomisi = $('.input-nama-komisi').val().replace('.', '');
            let nominalKomisi = $('.input-nominal-komisi').val().replace('.', '');

            if(namaKomisi.trim().length == 0 || nominalKomisi.trim().length == 0) {
                $('[autofocus]').focus();
                alert('Mohon lengkapi data komisi terlebih dahulu');
                return false;
            }

            // kalau indexKomisi tidak kosong, berarti update
            if(this.indexKomisi !== '') {
                this.datas.komisi[this.indexKomisi].nama = namaKomisi;
                this.datas.komisi[this.indexKomisi].nominal = nominalKomisi;
            }

            // kalau kosong, berarti tambah baru
            else {
                this.datas.komisi.push({
                    nama: namaKomisi,
                    nominal: nominalKomisi
                })
            }

            this.hideModal('#modalKomisi');
        },
        deleteKomisi() {
            let confirmDelete = confirm('Anda yakin ingin menghapus komisi ini?');
            if(confirmDelete == true) {
                this.datas.komisi.splice(this.indexKomisi, 1);
                this.hideModal('#modalKomisi');

                return;
            } else {
                $('.btn-delete-komisi').blur();
            }
        },
        updateTanggungan() {
            let namaTanggungan = $('.input-nama-tanggungan').val();
            let nominalTanggungan = $('.input-nominal-tanggungan').val().replace('.', '');
            let keteranganTanggungan = $('.input-keterangan-tanggungan').val();

            if(namaTanggungan.trim().length == 0 || nominalTanggungan.trim().length == 0 || keteranganTanggungan.trim().length == 0) {
                $('[autofocus]').focus();
                alert('Mohon lengkapi data tanggungan terlebih dahulu');
                return false;
            }

            // kalau indexTanggungan tidak kosong, berarti update
            if(this.indexTanggungan !== '') {
                this.datas.tanggungan[this.indexTanggungan].nama = namaTanggungan;
                this.datas.tanggungan[this.indexTanggungan].nominal = nominalTanggungan;
                this.datas.tanggungan[this.indexTanggungan].keterangan = keteranganTanggungan;
            }

            // kalau kosong, berarti tambah baru
            else {
                this.datas.tanggungan.push({
                    nama: namaTanggungan,
                    nominal: nominalTanggungan,
                    keterangan: keteranganTanggungan
                })
            }

            this.hideModal('#modalTanggungan');
        },
        deleteTanggungan() {
            let confirmDelete = confirm('Anda yakin ingin menghapus tanggungan ini?');
            if(confirmDelete == true) {
                this.datas.tanggungan.splice(this.indexTanggungan, 1);
                this.hideModal('#modalTanggungan');

                return;
            } else {
                $('.btn-delete-tanggungan').blur();
            }
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
        fetchListBank() {
            let self = this;
            $.ajax({
                url: 'https://boss.smartlink.id/salary/bank',
                type: 'GET',
                dataType: 'JSON',
                success: function(res) {
                    let data = res.data;
                    self.listRekening = data;
                },
                fail: function(err) {
                    console.error('Error fetching bank list : ', err);
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
        submitGaji() {
            if(isNaN(this.selectedRekening)) {
                $('#namaBank').focus();
                return alert('Mohon pilih rekening terlebih dahulu');
            } else if(!this.tanggalGaji) {
                $('#tanggal').focus();
                return alert('Mohon pilih tanggal gaji dahulu');
            }

            let getBank = this.listRekening[this.selectedRekening];
            let copyData = this.datas;
            delete copyData.upah_borongan;

            let dataGaji = {
                rekening: getBank,
                tanggal_catat: this.tanggalGaji,
                keterangan: this.keteranganGaji
            }

            let sentData = { ...copyData, ...dataGaji  }
            console.log('sentData : ', JSON.stringify(sentData));

            $.ajax({
                url: 'https://boss.smartlink.id/salary/save',
                type: 'POST',
                dataType: 'JSON',
                headers: {
                    'Accept': 'application/json',
                },
                data: JSON.stringify(sentData),
                success: function(res) {
                    console.log('Sukses :  ', res)
                },
                error: function(err) {
                    console.error('Error : ', err);
                }
            });
        }
    },
    watch: {
        stepPenggajian(newVal) {
            if(newVal == 2) {
                $('.prev-step').show();
                $('.nav-title').text('Detail Pembayaran');

                if(!this.alreadyFetchBank) {
                    this.fetchListBank();
                }
            } else {
                $('.prev-step').hide();
                $('.nav-title').text('Faktur Gaji')
            }
        }
    },
    mounted() {
        var self = this;

        $('.modal-overlay').hide();
        $('.modal-custom').hide();
        self.fetchData();

        $('.input-angka').on('input', function() {
            let inputVal = $(this).val();
            if(isNaN(inputVal)) {
                $(this).val('');
                return;
            }

            const cleanVal = inputVal.replace('.', '');
            $(this).val(parseInt(cleanVal).toLocaleString('id'));
        });

        $('.modal-overlay').click(function() {
            self.hideModal();
        });

        $('.prev-step').click(function() {
            self.stepPenggajian--;
        })

        self.stepPenggajian = 1;
    }
}

</script>