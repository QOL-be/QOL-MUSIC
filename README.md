
package com.qol.music

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

data class Song(
    val title: String,
    val artist: String,
    val uri: String = ""
)

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        setContent {
            QOLMusicApp()
        }
    }
}

@Composable
fun QOLMusicApp() {
    var songs by remember {
        mutableStateOf(
            listOf(
                Song("My Music", "QOL MUSIC"),
                Song("Offline Player", "Local Library")
            )
        )
    }

    var selectedSong by remember {
        mutableStateOf<Song?>(null)
    }

    MaterialTheme(
        colorScheme = darkColorScheme()
    ) {
        Surface(
            modifier = Modifier.fillMaxSize(),
            color = MaterialTheme.colorScheme.background
        ) {
            Column(
                modifier = Modifier
                    .fillMaxSize()
                    .padding(20.dp)
            ) {
                Text(
                    text = "QOL MUSIC",
                    style = MaterialTheme.typography.headlineLarge
                )

                Text(
                    text = "Your personal music space",
                    style = MaterialTheme.typography.bodyMedium
                )

                Spacer(modifier = Modifier.height(25.dp))

                Text(
                    text = "My Library",
                    style = MaterialTheme.typography.titleLarge
                )

                LazyColumn(
                    modifier = Modifier.weight(1f)
                ) {
                    items(songs) { song ->
                        Card(
                            modifier = Modifier
                                .fillMaxWidth()
                                .padding(vertical = 5.dp),
                            onClick = {
                                selectedSong = song
                            }
                        ) {
                            Row(
                                modifier = Modifier.padding(16.dp),
                                verticalAlignment = Alignment.CenterVertically
                            ) {
                                Column(
                                    modifier = Modifier.weight(1f)
                                ) {
                                    Text(song.title)
                                    Text(
                                        song.artist,
                                        style = MaterialTheme.typography.bodySmall
                                    )
                                }

                                Text("▶")
                            }
                        }
                    }
                }

                selectedSong?.let {
                    Text("Selected: ${it.title}")
                }

                Button(
                    onClick = {
                        songs = songs + Song(
                            "New Song ${songs.size + 1}",
                            "My Library"
                        )
                    },
                    modifier = Modifier.fillMaxWidth()
                ) {
                    Text("Add to library")
                }
            }
        }
    }
}
