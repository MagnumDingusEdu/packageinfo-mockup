<template>
  <v-app id="inspire">
    <v-app-bar
        app
        flat
        id="toolbar"
    >
      <v-toolbar-title class="">
        PACKAGE<span class="font-weight-black">INFO</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items class="hidden-sm-and-down">
        <v-btn
            v-for="link in links"
            :key="link"
            text
        >
          {{ link }}
        </v-btn>
      </v-toolbar-items>
      <v-menu
          bottom
          left

      >
        <template v-slot:activator="{ on, attrs }">
          <v-btn
              dark
              icon
              v-bind="attrs"
              v-on="on"
              :style="{display: showMenu }"
          >
            <v-icon>mdi-menu</v-icon>
          </v-btn>
        </template>

        <v-list>
          <v-list-item
              v-for="link in links"
              :key="link"
          >
            <v-list-item-title class="text-capitalize">{{ link }}</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>
    </v-app-bar>

    <v-main>
      <v-container>
        <v-row>
          <v-col cols="12" md="8">
            <v-text-field
                placeholder="Search for a package"
                outlined
                append-icon="mdi-magnify"

                clearable
            />
          </v-col>
          <v-col cols="12" md="4">
            <v-combobox
                v-model="selectedDistros"
                :items="availableDistros"
                label="Select your distribution"
                clearable
                outlined
            >
              <template v-slot:selection="data">

                {{ data.item }}

              </template>
            </v-combobox>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12" md="3">
            <v-sheet rounded="lg">
              <v-list>
                <v-list-item>
                  <v-list-item-avatar rounded>
                    <v-img src="../assets/ubuntuicon.png" contain></v-img>
                  </v-list-item-avatar>
                </v-list-item>

                <v-list-item link>
                  <v-list-item-content>
                    <v-list-item-title class="title">
                      Ubuntu Linux
                    </v-list-item-title>

                  </v-list-item-content>


                </v-list-item>
              </v-list>
              <v-divider></v-divider>

              <v-list
                  nav
                  dense
              >
                <v-list-item-group
                    v-model="distroVersion"
                    color="primary"
                >
                  <v-list-item
                      v-for="distro in distroVersions"
                      :key="distro"
                  >
                    <v-list-item-icon>
                      <v-icon>mdi-clock-time-four-outline</v-icon>
                    </v-list-item-icon>

                    <v-list-item-content>
                      <v-list-item-title v-text="distro"></v-list-item-title>
                    </v-list-item-content>
                  </v-list-item>
                </v-list-item-group>
              </v-list>
              <v-container>
                <v-combobox
                    v-model="selectedArch"
                    :items="arch_options"
                    label="Architecture"
                    multiple
                    outlined
                ></v-combobox>
                <v-combobox
                    v-model="selectedRepository"
                    :items="repo_options"
                    label="Repository"
                    multiple
                    outlined
                ></v-combobox>
              </v-container>

            </v-sheet>
          </v-col>

          <v-col>
            <v-sheet
                rounded="lg"
            >
              <v-data-table
                  :headers="headers"
                  :items="packages"
                  :page.sync="page"
                  :items-per-page="itemsPerPage"
                  hide-default-footer
                  @page-count="pageCount = $event"
                  :sort-desc="[false, true]"
                  multi-sort
                  class="elevation-1"
                  @click:row="handleTableClick"
              ></v-data-table>

            </v-sheet>
            <div class="text-center pt-2">
              <v-pagination
                  v-model="page"
                  :length="pageCount"
              ></v-pagination>

            </div>
          </v-col>
        </v-row>
        <v-dialog
            v-model="dialog"
            max-width="70vh"
            overlay-opacity="0.9"

        >
          <v-card
              class="mx-auto"
          >
            <v-container>
              <v-row>
                <v-col cols="12" sm="8">
                  <v-card-title class="mb-2">{{ this.currentPackage.name }}</v-card-title>
                  <v-card-text>
                    <v-row
                        align="center"
                        class="mx-0"
                    >
                      Latest Version :

                      <div class="grey--text ml-1">
                        {{ this.currentPackage.version }}
                      </div>


                    </v-row>
                    <v-row
                        align="center"
                        class="mx-0"
                    >
                      Architecture :

                      <div class="grey--text ml-1">
                        {{ this.currentPackage.architecture }}
                      </div>

                    </v-row>
                    <v-row
                        align="center"
                        class="mx-0"
                    >
                      Download size :

                      <div class="grey--text ml-1">
                        {{ this.randomSize() }} KB
                      </div>

                    </v-row>
                  </v-card-text>
                </v-col>
                <v-spacer></v-spacer>
                <v-col class="hidden-sm-and-down justify-end mx-8">

                  <v-img src="../assets/ubuntuicon.png" width="100"></v-img>
                </v-col>
              </v-row>
            </v-container>
            <v-divider class="my-4 mx-4"></v-divider>
            <v-card-text>
              <p class="justify">This is a sample description. Lorem ipsum dolor sit amet, consectetur adipisicing elit.
                Amet animi dolores doloribus, ea illum impedit in libero maxime nulla odio perferendis, praesentium quam
                quis quos repellat similique velit vero voluptas.

              </p>
            </v-card-text>
            <v-divider class="mx-4"></v-divider>

            <v-card-title>Versions</v-card-title>
            <v-card-text>
              <v-chip-group
                  v-model="selection"
                  active-class="deep-purple accent-4 white--text"
                  column
              >
                <v-chip>0.0.23.1-1</v-chip>

                <v-chip>0.0.24</v-chip>

                <v-chip>0.0.24.1-1</v-chip>

                <v-chip>0.0.25</v-chip>
              </v-chip-group>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn

                  color="deep-orange lighten-2"
                  text


              >
                Download Source
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-container>
    </v-main>
    <v-footer>
      <v-col
          class="text-center grey--text caption"
          cols="12"

      >

        Database last updated on <strong> {{ new Date() }} | Mockup by Vividh Mariya </strong>

      </v-col>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: "Mockup",
  data: () => ({
    selection: null,
    dialog: false,
    selectedArch: null,
    selectedRepository: null,
    page: 1,
    pageCount: 0,
    itemsPerPage: 12,
    arch_options: [
      'amd64',
      'i686',
      'all'
    ],
    repo_options: [
      'core',
      'fossa',
      'ubuntu-extra'
    ],
    links: [
      'browse packages',
      'documentation',
      'github',
    ],
    selectedDistros: null,
    availableDistros: [
      'Ubuntu',
      'Fedora',
      'Arch Linux',
      'Debian',
      'CentOS'

    ],
    headers: [
      {text: 'Package name', align: 'start', value: 'name'},
      {text: 'Version', value: 'version'},
      {text: 'Repository', value: 'repository'},
      {text: 'Architecture', value: 'architecture'},
    ],
    packages: [
      {
        name: "0ad-data-common",
        version: "0.0.23.1-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "c756af1f-9186-4c28-907b-4f861044b833",
      }, {
        name: "0ad-data",
        version: "0.0.23.1-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "a2ea7f25-1402-4e71-988e-7e7becb9a001",
      }, {
        name: "0ad",
        version: "0.0.23.1-4ubuntu3",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "7ecdd5c8-70e2-494f-9488-0e298bd2a563",
      }, {
        name: "0install-core",
        version: "2.15.1-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "dc004a77-57e3-4fe5-93f3-e0448525fcaf",
      }, {
        name: "0install",
        version: "2.15.1-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "aa92c775-6b0f-44ad-a10a-747bb0f9131e",
      }, {
        name: "0xffff",
        version: "0.8-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "59c29db1-f07d-4963-89d4-76dad6347449",
      }, {
        name: "2048-qt",
        version: "0.1.6-2build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "e1d1db9f-f22b-4250-9ad5-0911f4ca048b",
      }, {
        name: "2ping",
        version: "4.3-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "5d5159be-8598-46ea-8212-52c466e1796f",
      }, {
        name: "2to3",
        version: "3.8.2-0ubuntu2",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "75b649d3-edb7-4999-89fe-ab73a6d00600",
      }, {
        name: "2vcard",
        version: "0.6-2",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "a4f25257-36a6-4c53-bf65-6d7754fa1ac1",
      }, {
        name: "3270-common",
        version: "3.6ga4-3build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "4bfa02e2-065b-48cc-96ad-95c1b69e7df3",
      }, {
        name: "389-ds-base-dev",
        version: "1.4.3.6-2",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "97b75905-2bf3-4d34-a697-5009223cb889",
      }, {
        name: "389-ds-base-libs",
        version: "1.4.3.6-2",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "ebd612dd-72d5-430d-9091-7fbb2f1868cb",
      }, {
        name: "389-ds-base",
        version: "1.4.3.6-2",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "8aeebba9-0813-4a3a-b180-f5a4e63d3b9a",
      }, {
        name: "389-ds",
        version: "1.4.3.6-2",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "09c29025-d085-4094-a170-ba8c409facb8",
      }, {
        name: "3dchess",
        version: "0.8.1-20",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "3a57de53-eea4-47b9-916d-5fe267cd4f1c",
      }, {
        name: "3depict",
        version: "0.0.22-1.2build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "9a934677-ffa7-46f3-93e2-7909b7cc4860",
      }, {
        name: "3dldf-doc",
        version: "2.0.3+ndfsg-4",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "6522d184-5281-4a9d-a282-66a2ce1b39fa",
      }, {
        name: "4g8",
        version: "1.0-3.2",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "a64966f6-f2df-48af-93c8-a44839a535f5",
      }, {
        name: "4pane",
        version: "6.0-1build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "b3e5e64a-e13a-4769-ab07-d2fc5543f86b",
      }, {
        name: "4store",
        version: "1.1.6+20151109-2build2",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "7fcdfa21-508a-4cd3-88e9-357afacf4440",
      }, {
        name: "4ti2-doc",
        version: "1.6.9+ds-2build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "1cac4096-f215-4b6d-956e-b07f8f09f65a",
      }, {
        name: "4ti2",
        version: "1.6.9+ds-2build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "949136d8-35c7-4c8f-b221-2491527ec1d8",
      }, {
        name: "64tass",
        version: "1.54.1900-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "d49476b6-7d98-4141-8f13-604c4937edf0",
      }, {
        name: "6tunnel",
        version: "1:0.13-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "ef09e6ab-a0a9-4e4e-9e2d-b7f865a217b1",
      }, {
        name: "7kaa-data",
        version: "2.15.2+dfsg-2build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "ed457f5c-0675-42fe-be19-d4262c48649d",
      }, {
        name: "7kaa",
        version: "2.15.2+dfsg-2build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "72fecbc8-7c58-40dd-99cd-c002f00f0c25",
      }, {
        name: "9base",
        version: "1:6-7build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "bf14078e-fcd5-4922-ba89-b8325e79db51",
      }, {
        name: "9menu",
        version: "1.9-2",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "3aba6eac-32c1-49e3-8008-7ea7eb8d201e",
      }, {
        name: "9mount",
        version: "1.3+hg20170412-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "ae177cb5-2c53-401e-911a-f41fe9b3bd7e",
      }, {
        name: "9wm",
        version: "1.4.1-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "4bb9a53f-02b9-449d-ab1e-803014cbe99a",
      }, {
        name: "a11y-profile-manager-doc",
        version: "0.1.11-0ubuntu4",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "e867224b-cbb3-44aa-8321-cd619825a180",
      }, {
        name: "a11y-profile-manager-indicator",
        version: "0.1.11-0ubuntu4",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "30f089be-f804-4729-99d4-96a064369acd",
      }, {
        name: "a11y-profile-manager",
        version: "0.1.11-0ubuntu4",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "cf7ce8da-781f-4bb5-865e-5f8d9284ec9c",
      }, {
        name: "a2jmidid",
        version: "9-2",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "2b56a1d2-1818-4d52-b31c-8704a8b3f77d",
      }, {
        name: "a2ps",
        version: "1:4.14-5",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "1b63d149-930d-4230-b54b-1c0d78629533",
      }, {
        name: "a56",
        version: "1.3+dfsg-9",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "89d7581f-d136-4f04-a11a-53a367d8d3b7",
      }, {
        name: "a7xpg-data",
        version: "0.11.dfsg1-10build3",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "7ac69032-caf3-4e04-8e93-920bea7b6e76",
      }, {
        name: "a7xpg",
        version: "0.11.dfsg1-10build3",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "3cf29645-7f59-4cdd-b2e3-a7780b181c7f",
      }, {
        name: "aa3d",
        version: "1.0-8build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "e80134b5-2c54-4b3c-8f48-747f674d51e5",
      }, {
        name: "aac-enc",
        version: "0.1.6-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "04022f51-76f2-4ce6-9680-f8f0035b38cf",
      }, {
        name: "aajm",
        version: "0.4-10build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "4becb245-e633-45e5-be20-509180161410",
      }, {
        name: "aaphoto",
        version: "0.45-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "66ccf56b-76a1-4596-9e5c-5853dfb2c876",
      }, {
        name: "aapt",
        version: "1:8.1.0+r23-3build2",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "baddf43a-2ac2-4172-b918-1d2aed8203e5",
      }, {
        name: "abacas",
        version: "1.3.1-6",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "92fb5d5a-4323-44a3-ab87-131a77769a4c",
      }, {
        name: "abcde",
        version: "2.9.3-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "980189c3-0a39-46de-a58c-579c0780d4fc",
      }, {
        name: "abci",
        version: "0.0~git20170124.0.f94ae5e-2",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "339c3e36-0f58-423c-a05e-bf5ee931f52a",
      }, {
        name: "abcm2ps",
        version: "8.14.6-0.1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "58302b68-94d2-4c50-aa17-03ee1a337f80",
      }, {
        name: "abcmidi",
        version: "20200122-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "aac033a3-952c-4fce-b7d4-c44061b7a5c1",
      }, {
        name: "abe-data",
        version: "1.1+dfsg-3",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "6b0b67d3-2361-43dc-ae48-cdd691c08b15",
      }, {
        name: "abe",
        version: "1.1+dfsg-3",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "100088de-ba9e-40c9-bf04-cebfe9ae1930",
      }, {
        name: "abgate",
        version: "1.1.9-2build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "d2b52695-fca7-4f98-ba0e-65b9591d3c5f",
      }, {
        name: "abi-compliance-checker",
        version: "2.3-0.2ubuntu1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "5957a2ab-78b6-4e93-9e9a-85670641f9db",
      }, {
        name: "abi-dumper",
        version: "1.1-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "f11529ff-1f70-4ae4-9d88-8f99b8b51644",
      }, {
        name: "abi-monitor",
        version: "1.12-2ubuntu1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "f21c67f1-4d9e-4e57-8bec-2a8a9957240f",
      }, {
        name: "abi-tracker",
        version: "1.11-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "fc7c410e-5b86-4cd6-8598-bdda3f91ac3b",
      }, {
        name: "abicheck",
        version: "1.2-5ubuntu1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "05a9e425-f382-40ad-b100-3d6546a02121",
      }, {
        name: "abigail-doc",
        version: "1.6-1build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "e46ff517-2675-488a-b909-45f04d9ebada",
      }, {
        name: "abigail-tools",
        version: "1.6-1build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "87e05d96-f07c-41f9-b1d0-59c981c300cf",
      }, {
        name: "abisip-find",
        version: "1.3.0-2",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "0e7eec87-008c-43d0-9bf4-874b70640a6a",
      }, {
        name: "abiword-common",
        version: "3.0.2-10",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "1f9688ae-2c6b-411e-9155-17de1a82e783",
      }, {
        name: "abiword-plugin-grammar",
        version: "3.0.2-10",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "ee44c2d9-ece0-43c7-85db-7709cbbc0176",
      }, {
        name: "abiword",
        version: "3.0.2-10",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "ed92a0b8-4e09-44dc-afc0-b6d79625060f",
      }, {
        name: "ableton-link-dev",
        version: "3.0.2+dfsg-1build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "4e79092d-e814-442e-8b62-411833b99020",
      }, {
        name: "ableton-link-utils-gui",
        version: "3.0.2+dfsg-1build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "2e98eca4-658b-4fe0-b13d-cbb8423e3701",
      }, {
        name: "ableton-link-utils",
        version: "3.0.2+dfsg-1build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "6150207b-93ba-4c65-bb97-e452d0ec5d24",
      }, {
        name: "abntex",
        version: "0.9~beta2-6",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "f2b3da47-fe97-4ebd-803e-61cef4df56d6",
      }, {
        name: "abook",
        version: "0.6.1-1build4",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "67799b85-0f69-4374-a905-471612fd8840",
      }, {
        name: "abootimg",
        version: "0.6-1build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "7988a0fc-cdab-4da3-ab2e-cf61394cac14",
      }, {
        name: "abr2gbr",
        version: "1:1.0.2-2.1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "80ffce7a-0319-43b9-acc5-f115a96f8009",
      }, {
        name: "abs-guide",
        version: "10-3",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "c3574f0d-77df-487e-8520-f7125627a600",
      }, {
        name: "abw2epub",
        version: "0.9.6-2build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "61373668-f6f5-4fb0-9e03-ad8ee715ef83",
      }, {
        name: "abw2odt",
        version: "0.9.6-2build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "95505371-83a5-4aea-bd10-25636a301464",
      }, {
        name: "abx",
        version: "0.0~b1-1build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "8ccf4582-7506-498a-aecf-265d0c184daa",
      }, {
        name: "abyss",
        version: "2.2.4-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "10c78c34-55ec-4b98-bb31-e4bc4f3af04d",
      }, {
        name: "acbuild",
        version: "0.4.0+dfsg-3",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "a2caa16f-56a0-4046-bcc7-26638c82878b",
      }, {
        name: "accerciser",
        version: "3.36.0-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "fef306ac-9336-4173-88d2-28d42597614b",
      }, {
        name: "accounts-qml-module-doc",
        version: "0.6+17.04.20170405-0ubuntu4",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "5082241a-f2f1-4819-a86e-b874b0697c20",
      }, {
        name: "accountsservice-ubuntu-schemas",
        version: "0.0.7+17.10.20170922-0ubuntu1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "3c1cb469-852f-456f-bdee-d9ca4a564d8a",
      }, {
        name: "accountsservice-ubuntu-touch-schemas",
        version: "0.0.7+17.10.20170922-0ubuntu1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "abc9fd20-6757-4853-80c8-37b0ab0fb007",
      }, {
        name: "accountsservice",
        version: "0.6.55-0ubuntu12~20.04.4",
        repository: "focal-updates,focal-security",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "75c9b01d-b7bd-4b94-a1cd-f5de548e8fb1",
      }, {
        name: "accountwizard",
        version: "4:19.12.3-0ubuntu1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "084f13fc-19a7-482f-b0e9-c646efdd0667",
      }, {
        name: "acct",
        version: "6.6.4-2",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "8a913ade-9ad5-46e3-8609-63165a5a2e12",
      }, {
        name: "ace-gperf",
        version: "6.4.5+dfsg-1build4",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "2406b37a-9dd5-4d4e-9fd9-610c71872848",
      }, {
        name: "ace-netsvcs",
        version: "6.4.5+dfsg-1build4",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "9dcf44fc-3e1d-4e2a-9e23-e6e710819507",
      }, {
        name: "ace-of-penguins",
        version: "1.5~rc2-3",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "4760b1bc-f3c9-4880-9084-839a570aa45f",
      }, {
        name: "ace",
        version: "0.0.5-3",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "187b4c06-8553-4c69-9467-1c875645e340",
      }, {
        name: "acedb-other-belvu",
        version: "4.9.39+dfsg.02-4build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "d8421c80-4274-443e-b666-7dcc39c290e4",
      }, {
        name: "acedb-other-dotter",
        version: "4.9.39+dfsg.02-4build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "b5af2dc7-d138-4444-a49e-24f948255c0a",
      }, {
        name: "acedb-other",
        version: "4.9.39+dfsg.02-4build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "b9514899-e3fd-426a-a1d7-3d16e5b99de8",
      }, {
        name: "aces3-data",
        version: "3.0.8-6build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "b6625d93-3ff4-4f12-9091-d1d0f9d18a37",
      }, {
        name: "aces3",
        version: "3.0.8-6build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "8e0b7ade-f250-4bbb-8282-802b2b787813",
      }, {
        name: "acetoneiso",
        version: "2.4-3build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "ed74ed66-258d-4067-95e8-da5beb47780e",
      }, {
        name: "acfax",
        version: "981011-17build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "bb53cda5-3928-47e8-bd7e-cec3aa4798eb",
      }, {
        name: "acheck-rules",
        version: "0.3.5",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "076b2d62-2140-41b5-a559-9d6b3254a275",
      }, {
        name: "acheck",
        version: "0.5.8",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "e03e26b3-4beb-4b1a-8c00-365b2bc0d4e8",
      }, {
        name: "achilles",
        version: "2-9build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
        id: "01e85eea-d7cd-4211-9be4-c1b0ec069670",
      }, {
        name: "acidrip",
        version: "0.14-0.2ubuntu8",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "d9b89ee6-dd57-49c2-b036-64523ef4fe02",
      }, {
        name: "ack",
        version: "3.3.1-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "df41d1a6-c4fc-4f6a-88e5-7e3b1a3dafa0",
      }, {
        name: "acl2-books-certs",
        version: "8.2dfsg-3build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
        id: "9793ed53-3681-4b2b-b401-4e021b302204",
      }
    ],
    distroVersion: null,
    distroVersions: [
      'Ubuntu 20.10',
      'Ubuntu 20.04 LTS',
      'Ubuntu 19.10',
      'Ubuntu 19.04',
      'Ubuntu 18.04 LTS'
    ],
    currentPackage: {
      name: "0ad-data-common",
      version: "0.0.23.1-1",
      repository: "focal",
      distribution: "20.04 Focal Fossa",
      architecture: "all",
      id: "c756af1f-9186-4c28-907b-4f861044b833",
    },
  }),
  computed: {
    showMenu() {
      if (this.$vuetify.breakpoint.mdAndUp) {
        return 'none'
      } else {
        return 'block'
      }

    }
  },
  mounted() {
    this.currentPackage = this.packages[0]
  },
  methods: {
    handleTableClick(value) {
      this.dialog = true;
      this.currentPackage = value;
    },
    randomSize() {
      const precision = 100;
      return Math.floor(Math.random() * (100 * precision - precision) + precision) / (precision);
    }
  }
}
</script>

<style scoped>

</style>