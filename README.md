# 01
// ARViewScreen.js
import React, { useState, useEffect } from 'react';
import { View, Text, StyleSheet } from 'react-native';
import { Camera } from 'expo-camera';

export default function ARViewScreen() {
  const [hasPermission, setHasPermission] = useState(null);

  useEffect(() => {
    (async () => {
      const { status } = await Camera.requestCameraPermissionsAsync();
      setHasPermission(status === 'granted');
    })();
  }, []);

  if (hasPermission === null) return <View />;
  if (hasPermission === false) return <Text>No access to camera</Text>;

  return (
    <View style={styles.container}>
      <Camera style={styles.camera}>
        <Text style={styles.overlayText}>🌬️ Spirit of the Wind Appears!</Text>
      </Camera>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1 },
  camera: { flex: 1, justifyContent: 'center', alignItems: 'center' },
  overlayText: {
    color: 'white', fontSize: 20, fontWeight: 'bold', backgroundColor: 'rgba(0,0,0,0.5)', padding: 10,
  },
});

// MapViewScreen.js
import React from 'react';
import { View, StyleSheet } from 'react-native';
import MapView, { Marker } from 'react-native-maps';

export default function MapViewScreen() {
  const spots = [
    { id: 1, title: '神社前スポット', latitude: 37.8025, longitude: 138.2431 },
    { id: 2, title: '岬の絶景スポット', latitude: 37.8045, longitude: 138.2500 },
  ];

  return (
    <MapView
      style={styles.map}
      initialRegion={{
        latitude: 37.8025,
        longitude: 138.2431,
        latitudeDelta: 0.01,
        longitudeDelta: 0.01,
      }}
    >
      {spots.map(spot => (
        <Marker
          key={spot.id}
          coordinate={{ latitude: spot.latitude, longitude: spot.longitude }}
          title={spot.title}
        />
      ))}
    </MapView>
  );
}

const styles = StyleSheet.create({
  map: { flex: 1 },
});

// CollectionScreen.js
import React, { useState } from 'react';
import { View, Text, FlatList, TouchableOpacity, StyleSheet, Modal } from 'react-native';

const characterData = [
  { id: '1', name: '風の精', description: '佐渡の風をまとった妖精キャラ', region: '佐渡' },
  { id: '2', name: '火山の守り神', description: '火山とともに生きる伝説の獣', region: '阿蘇' },
];

export default function CollectionScreen() {
  const [selectedChar, setSelectedChar] = useState(null);

  return (
    <View style={styles.container}>
      <Text style={styles.title}>ご当地キャラ図鑑</Text>
      <FlatList
        data={characterData}
        keyExtractor={item => item.id}
        renderItem={({ item }) => (
          <TouchableOpacity onPress={() => setSelectedChar(item)}>
            <Text style={styles.item}>{item.name}（{item.region}）</Text>
          </TouchableOpacity>
        )}
      />

      <Modal visible={!!selectedChar} transparent={true} animationType="slide">
        <View style={styles.modal}>
          <Text style={styles.title}>{selectedChar?.name}</Text>
          <Text>{selectedChar?.description}</Text>
          <TouchableOpacity onPress={() => setSelectedChar(null)}>
            <Text style={styles.close}>閉じる</Text>
          </TouchableOpacity>
        </View>
      </Modal>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { padding: 20, flex: 1, backgroundColor: '#fff' },
  title: { fontSize: 20, fontWeight: 'bold', marginBottom: 10 },
  item: { fontSize: 16, paddingVertical: 8 },
  modal: {
    marginTop: 200, marginHorizontal: 30, padding: 20,
    backgroundColor: '#fff', borderRadius: 10, elevation: 5,
  },
  close: { marginTop: 20, color: 'blue', fontWeight: 'bold' }
});
