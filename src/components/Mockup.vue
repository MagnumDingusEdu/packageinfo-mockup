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
        name: "acl",
        version: "2.3.1-1",
        repository: "core",
        distribution: "Arch Linux",
        architecture: "all",
      }, {
        name: "amd-ucode",
        version: "20210315.3568f96-1",
        repository: "core",
        distribution: "Arch Linux",
        architecture: "all",
      }, {
        name: "archlinux-keyring",
        version: "20210110-1",
        repository: "core",
        distribution: "Arch Linux",
        architecture: "all",
      }, {
        name: "argon2",
        version: "20190702-3",
        repository: "core",
        distribution: "Arch Linux",
        architecture: "all",
      }, {
        name: "attr",
        version: "2.5.1-1",
        repository: "core",
        distribution: "Arch Linux",
        architecture: "all",
      }, {
        name: "audit",
        version: "3.0.1-1",
        repository: "core",
        distribution: "Arch Linux",
        architecture: "all",
      }, {
        name: "autoconf",
        version: "2.71-1",
        repository: "core",
        distribution: "Arch Linux",
        architecture: "all",
      }, {
        name: "automake",
        version: "1.16.3-1",
        repository: "core",
        distribution: "Arch Linux",
        architecture: "all",
      }, {
        name: "b43-fwcutter",
        version: "019-3",
        repository: "core",
        distribution: "Arch Linux",
        architecture: "all",
      }, {
        name: "base",
        version: "2-2",
        repository: "core",
        distribution: "Arch Linux",
        architecture: "all",
      }, {
        name: "bash",
        version: "5.1.004-1",
        repository: "core",
        distribution: "Arch Linux",
        architecture: "all",
      }, {
        name: "binutils",
        version: "2.36.1-2",
        repository: "core",
        distribution: "Arch Linux",
        architecture: "all",
      }, {
        name: "bison",
        version: "3.7.6-1",
        repository: "core",
        distribution: "Arch Linux",
        architecture: "all",
      }, {
        name: "btrfs-progs",
        version: "5.11.1-1",
        repository: "core",
        distribution: "Arch Linux",
        architecture: "all",
      }, {
        name: "bzip2",
        version: "1.0.8-4",
        repository: "core",
        distribution: "Arch Linux",
        architecture: "all",
      }, {
        name: "ca-certificates",
        version: "20181109-4",
        repository: "core",
        distribution: "Arch Linux",
        architecture: "all",
      }, {
        name: "ca-certificates-mozilla",
        version: "3.63-1",
        repository: "core",
        distribution: "Arch Linux",
        architecture: "all",
      }, {
        name: "ca-certificates-utils",
        version: "20181109-4",
        repository: "core",
        distribution: "Arch Linux",
        architecture: "all",
      }, {
        name: "coreutils",
        version: "8.32-1",
        repository: "core",
        distribution: "Arch Linux",
        architecture: "all",
      }, {name: "cracklib", version: "2.9.7-2", repository: "core", distribution: "Arch Linux", architecture: "all",},
      {
        name: "0ad-data-common",
        version: "0.0.23.1-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
      }, {
        name: "0ad-data",
        version: "0.0.23.1-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
      }, {
        name: "0ad",
        version: "0.0.23.1-4ubuntu3",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
      }, {
        name: "0install-core",
        version: "2.15.1-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
      }, {
        name: "0install",
        version: "2.15.1-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
      }, {
        name: "0xffff",
        version: "0.8-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
      }, {
        name: "2048-qt",
        version: "0.1.6-2build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
      }, {
        name: "2ping",
        version: "4.3-1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
      }, {
        name: "2to3",
        version: "3.8.2-0ubuntu2",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
      }, {
        name: "2vcard",
        version: "0.6-2",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
      }, {
        name: "3270-common",
        version: "3.6ga4-3build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
      }, {
        name: "389-ds-base-dev",
        version: "1.4.3.6-2",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
      }, {
        name: "389-ds-base-libs",
        version: "1.4.3.6-2",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
      }, {
        name: "389-ds-base",
        version: "1.4.3.6-2",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
      }, {
        name: "389-ds",
        version: "1.4.3.6-2",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
      }, {
        name: "3dchess",
        version: "0.8.1-20",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
      }, {
        name: "3depict",
        version: "0.0.22-1.2build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
      }, {
        name: "3dldf-doc",
        version: "2.0.3+ndfsg-4",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "all",
      }, {
        name: "4g8",
        version: "1.0-3.2",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
      }, {
        name: "4pane",
        version: "6.0-1build1",
        repository: "focal",
        distribution: "20.04 Focal Fossa",
        architecture: "amd64",
      },


    ],
    distroVersion: null,
    distroVersions: [
      'Ubuntu 20.10',
      'Ubuntu 20.04 LTS',
      'Ubuntu 19.10',
      'Ubuntu 19.04',
      'Ubuntu 18.04 LTS'
    ]
  }),
  computed: {
    showMenu() {
      if (this.$vuetify.breakpoint.mdAndUp) {
        return 'none'
      } else {
        return 'block'
      }

    }
  }
}
</script>

<style scoped>

</style>